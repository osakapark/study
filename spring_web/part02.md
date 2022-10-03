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
