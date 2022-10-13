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
  
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head th:replace="fragments.html :: head"></head>

<body class="bg-light">
	<div th:replace="fragments.html :: main-nav"></div>
	<div class="container">
		<div class="row mt-5 justify-content-center">
			<div class="col-2">
				<div th:replace="fragments.html :: settings-menu(currentMenu='profile')"></div>
			</div>
			<div class="col-8">
				<div th:if="${message}" class="alert alert-warning alert-dismissible fade show" role="alert">
					<span th:text="${message}">메시지
					</span>
					<button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close">
					</button>
				</div>

				<div class="row">
					<h2 class="col-sm-12" th:text="${account.nickname}">whiteship</h2>
				</div>

				<div class="row mt-3">
					<form class="col-sm-6" action="#" th:action="@{/settings/profile}" th:object="${profile}"
						method="post" novalidate>
						<div class="form-group">
							<label for="bio">한 줄 소개</label>
							<input id="bio" type="text" th:field="*{bio}" class="form-control"
								placeholder="간략한 소개를 부탁합니다." aria-describedby="bioHelp" required>
							<small id="bioHelp" class="form-text text-muted">
								길지 않게 35자 이내로 입력하세요.
							</small>
							<small class="form-text text-danger" th:if="${#fields.hasErrors('bio')}" th:errors="*{bio}">
								조금 길어요.
							</small>
						</div>

						<div class="form-group">
							<label for="url">링크</label>
							<input id="url" type="url" th:field="*{url}" class="form-control"
								placeholder="http://studyolle.com" aria-describedby="urlHelp" required>
							<small id="urlHelp" class="form-text text-muted">
								블로그, 유튜브 또는 포트폴리오나 좋아하는 웹 사이트 등 본인을 표현할 수 있는 링크를 추가하세요.
							</small>
							<small class="form-text text-danger" th:if="${#fields.hasErrors('url')}" th:errors="*{url}">
								옳바른 URL이 아닙니다. 예시처럼 입력해 주세요.
							</small>
						</div>

						<div class="form-group">
							<label for="company">직업</label>
							<input id="company" type="text" th:field="*{occupation}" class="form-control"
								placeholder="어떤 일을 하고 계신가요?" aria-describedby="occupationHelp" required>
							<small id="occupationHelp" class="form-text text-muted">
								개발자? 매니저? 취준생? 대표님?
							</small>
						</div>

						<div class="form-group">
							<label for="location">활동 지역</label>
							<input id="location" type="text" th:field="*{location}" class="form-control"
								placeholder="Redmond, WA, USA" aria-describedby="locationdHelp" required>
							<small id="locationdHelp" class="form-text text-muted">
								주요 활동(사는 곳이나 직장을 다니는 곳 또는 놀러 다니는 곳) 지역의 도시 이름을 알려주세요.
							</small>
						</div>

						<div class="form-group">
							<input id="profileImage" type="hidden" th:field="*{profileImage}" class="form-control" />
						</div>

						<div class="form-group">
							<button class="btn btn-primary btn-block" type="submit" aria-describedby="submitHelp">수정하기
							</button>
						</div>
					</form>

					<div class="col-sm-6">
						<div class="card text-center">
							<div class="card-header">
								프로필 이미지
							</div>
							<div id="current-profile-image" class="mt-3">
								<svg th:if="${#strings.isEmpty(profile.profileImage)}" class="rounded"
									th:data-jdenticon-value="${account.nickname}" width="125" height="125">
								</svg>
								<img th:if="${!#strings.isEmpty(profile.profileImage)}" class="rounded"
									th:src="${profile.profileImage}" width="125" height="125" alt="name"
									th:alt="${account.nickname}" />
							</div>
							<div id="new-profile-image" class="mt-3">
							</div>

							<div class="card-body">
								<div class="input-group">
									<input type="file" class="form-control" id="profile-image-file"
										aria-describedby="inputGroupFileAddon04" aria-label="Upload">
								</div>

								<div id="new-profile-image-control" class="d-grid gap-2 mt-3">
									<button class="btn btn-outline-primary btn-block" id="cut-button">자르기</button>
									<button class="btn btn-outline-success btn-block" id="confirm-button">확인</button>
									<button class="btn btn-outline-warning btn-block" id="reset-button">취소</button>
								</div>
								<div id="cropped-new-profile-image" class="mt-3">
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>

	<link href="/node_modules/cropper/dist/cropper.min.css" rel="stylesheet">
	<script src="/node_modules/cropper/dist/cropper.min.js"></script>
	<script src="/node_modules/jquery-cropper/dist/jquery-cropper.min.js"></script>
	<script type="application/javascript">
		$(function () {
			cropper = '';
			let $confirmBtn = $("#confirm-button");
			let $resetBtn = $("#reset-button");
			let $cutBtn = $("#cut-button");
			let $newProfileImage = $("#new-profile-image");
			let $currentProfileImage = $("#current-profile-image");
			let $resultImage = $("#cropped-new-profile-image");
			let $profileImage = $("#profileImage");

			$newProfileImage.hide();
			$cutBtn.hide();
			$resetBtn.hide();
			$confirmBtn.hide();

			$("#profile-image-file").change(function (e) {
				if (e.target.files.length === 1) {
					const reader = new FileReader();
					reader.onload = e => {
						if (e.target.result) {
							let img = document.createElement("img");
							img.id = 'new-profile';
							img.src = e.target.result;
							img.width = 250;

							$newProfileImage.html(img);
							$newProfileImage.show();
							$currentProfileImage.hide();

							let $newImage = $(img);
							$newImage.cropper({aspectRatio: 1});
							cropper = $newImage.data('cropper');

							$cutBtn.show();
							$confirmBtn.hide();
							$resetBtn.show();
						}
					};
					reader.readAsDataURL(e.target.files[0]);
				}
			});

			$resetBtn.click(function () {
				$currentProfileImage.show();
				$newProfileImage.hide();
				$resultImage.hide();
				$resetBtn.hide();
				$cutBtn.hide();
				$confirmBtn.hide();
				$profileImage.val('');
			});

			$cutBtn.click(function () {
				let dataUrl = cropper.getCroppedCanvas().toDataURL();
				let newImage = document.createElement("img");
				newImage.id = "cropped-new-profile-image";
				newImage.src = dataUrl;
				newImage.width = 125;
				$resultImage.html(newImage);
				$resultImage.show();
				$confirmBtn.show();
				$confirmBtn.click(function () {
					$newProfileImage.html(newImage);
					$cutBtn.hide();
					$confirmBtn.hide();
					$profileImage.val(dataUrl);
				});
			});
		});
	</script>
</body>

</html>
```
## 28.

## 29.

## 30. 

## 31. Model Mapper
Modelmapper
```gradle
	// https://mvnrepository.com/artifact/org.modelmapper/modelmapper
	implementation group: 'org.modelmapper', name: 'modelmapper', version: '3.1.0'
```
