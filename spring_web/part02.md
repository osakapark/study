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

Test  코드
```java
@SpringBootTest
@AutoConfigureMockMvc
class SettingsControllerTest {

    @Autowired MockMvc mockMvc;
    @Autowired AccountRepository accountRepository;

    @AfterEach
    void afterEach() {
        accountRepository.deleteAll();
    }

    @WithAccount("keesun")
    @DisplayName("프로필 수정 폼")
    @Test
    void updateProfileForm() throws Exception {
        mockMvc.perform(get(SettingsController.SETTINGS_PROFILE_URL))
                .andExpect(status().isOk())
                .andExpect(model().attributeExists("account"))
                .andExpect(model().attributeExists("profile"));
    }

    @WithAccount("keesun")
    @DisplayName("프로필 수정하기 - 입력값 정상")
    @Test
    void updateProfile() throws Exception {
        String bio = "짧은 소개를 수정하는 경우.";
        mockMvc.perform(post(SettingsController.SETTINGS_PROFILE_URL)
                .param("bio", bio)
                .with(csrf()))
                .andExpect(status().is3xxRedirection())
                .andExpect(redirectedUrl(SettingsController.SETTINGS_PROFILE_URL))
                .andExpect(flash().attributeExists("message"));

        Account keesun = accountRepository.findByNickname("keesun");
        assertEquals(bio, keesun.getBio());
    }

    @WithAccount("keesun")
    @DisplayName("프로필 수정하기 - 입력값 에러")
    @Test
    void updateProfile_error() throws Exception {
        String bio = "길게 소개를 수정하는 경우. 길게 소개를 수정하는 경우. 길게 소개를 수정하는 경우. 너무나도 길게 소개를 수정하는 경우. ";
        mockMvc.perform(post(SettingsController.SETTINGS_PROFILE_URL)
                .param("bio", bio)
                .with(csrf()))
                .andExpect(status().isOk())
                .andExpect(view().name(SettingsController.SETTINGS_PROFILE_VIEW_NAME))
                .andExpect(model().attributeExists("account"))
                .andExpect(model().attributeExists("profile"))
                .andExpect(model().hasErrors());

        Account keesun = accountRepository.findByNickname("keesun");
        assertNull(keesun.getBio());
    }
}
```


# 27. 프로필 이미지 변경
## 27.1 npm
* npm install cropper
* npm install jquery-cropper

## 27.2 data url
https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URLs
* data: 라는 접두어를 가진 URL로 파일을 문서에 내장 시킬때 사용할 수 있다.
* 이미지를 DataURL로 저장할 수 있다.
  
