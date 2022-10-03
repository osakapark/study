# 25.  프로필 수정 처리


SettingsController
```java
@PostMapping("/settings/profile")
public String updateProfile(@CurrentUser Account account, @Valid Profile profile, Errors errors,
                            Model model, RedirectAttributes attributes) {
    if (errors.hasErrors()) {
        model.addAttribute(account);
        return SETTINGS_PROFILE_VIEW_NAME;
    }

    accountService.updateProfile(account, profile);
    attributes.addFlashAttribute("message", "프로필을 수정했습니다.");
    return "redirect:" + SETTINGS_PROFILE_URL;
}
/*
@Valid  다음 Error  위치
profile, error 는 자동으로 model 에 넣어 줌

*/
```

Profile.java :  default 생성자 필요
```java
@Data
@NoArgsConstructor  //defalt  생성자
public class Profile {

    private String bio;

    private String url;

    private String occupation;

    private String location;

    public Profile(Account account) {
        this.bio = account.getBio();
        this.url = account.getUrl();
        this.occupation = account.getOccupation();
        this.location = account.getLocation();
    }
}
```

AccountService.java
```java
public void updateProfile(Account account, Profile profile) {
    account.setUrl(profile.getUrl());
    account.setOccupation(profile.getOccupation());
    account.setLocation(profile.getLocation());
    account.setBio(profile.getBio());
    // TODO 프로필 이미지
    accountRepository.save(account);
    // TODO 문제가 하나 더 남았습니다.
}
```    

# 26. Profile Update Test

## 26.1 인증된 사용자가 접근할 수 있는 기능 테스트하기
* 실제 DB에 저장되어 있는 정보에 대응하는 인증된 Authentication이 필요하다.
* @WithMockUser로는 처리할 수 없다.

## 26.2 인증된 사용자를 제공할 커스텀 애노테이션 만들기

## 26.3  source  
   
커스텀 애노테이션 생성
```java
package org.mystudy;
\\\
@Retention(RetentionPolicy.RUNTIME)
@WithSecurityContext(factory = WithAccountSecurityContextFacotry.class)
public @interface WithAccount {
    String value();
}
```
SecurityContextFactory 구현
```java
package org.mystudy;
@RequiredArgsConstructor
public class WithAccountSecurityContextFacotry implements WithSecurityContextFactory<WithAccount> {

    private final AccountService accountService;

    @Override
    public SecurityContext createSecurityContext(WithAccount withAccount) {
        String nickname = withAccount.value();

        SignUpForm signUpForm = new SignUpForm();
        signUpForm.setNickname(nickname);
        signUpForm.setEmail(nickname + "@email.com");
        signUpForm.setPassword("12345678");
        accountService.processNewAccount(signUpForm);

        UserDetails principal = accountService.loadUserByUsername(nickname);
        Authentication authentication = new UsernamePasswordAuthenticationToken(principal, principal.getPassword(), principal.getAuthorities());
        SecurityContext context = SecurityContextHolder.createEmptyContext();
        context.setAuthentication(authentication);
        return context;
    }
}

```
