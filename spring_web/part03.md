# 34. 관심주제, 지역정보

Account.java
```java
public class Account {
    @Id
    @GeneratedValue
    @Column(name = "account_id")
    private Long id;


    @OneToMany(mappedBy = "account")	
    private Set<AccountTag> accountTag;
}
```

Tag.java
```java
public class Tag {
	@Id
	@GeneratedValue
	@Column(name = "tag_id")
	private Long id;

	private String title;

	@OneToMany(mappedBy = "tag")
	private Set<AccountTag> accountTag;
}
```

Account.java
```java
public class AccountTag {
	@Id
	@GeneratedValue
	private Long id;

	@ManyToOne
	@JoinColumn(name = "account_id")
	private Account account;

	@ManyToOne
	@JoinColumn(name = "tag_id")
	private Tag tag;
}
```

# 36.  관심 주제 등록 뷰
* npm install @yaireo/tagify

tags.html
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<head th:replace="fragments.html :: head"></head>

<body class="bg-light">
	<div th:replace="fragments.html :: main-nav"></div>
	<div class="container">
		<div class="row mt-5 justify-content-center">
			<div class="col-2">
				<div th:replace="fragments.html :: settings-menu(currentMenu='tags')"></div>
			</div>
			<div class="col-8">
				<div class="row">
					<h2 class="col-12">관심있는 스터디 주제</h2>
				</div>
				<div class="row">
					<div class="col-12">
						<div class="alert alert-info" role="alert">
							참여하고 싶은 스터디 주제를 입력해 주세요. 해당 주제의 스터디가 생기면 알림을 받을 수 있습니다. 태그를 입력하고 콤마(,)
							또는 엔터를 입력하세요.
						</div>
						<input name='tags-outside' class='tagify--outside col-12' placeholder='write tags to add below'>
					</div>
				</div>
			</div>
		</div>
	</div>

	<script src="/node_modules/@yaireo/tagify/dist/tagify.min.js"></script>
	<script src="/node_modules/@yaireo/tagify/dist/tagify.polyfills.min.js"></script>
	<link href="/node_modules/@yaireo/tagify/dist/tagify.css" rel="stylesheet" type="text/css" />

	<script type="application/javascript" th:inline="javascript">
		$(function () {
			var csrfToken = /*[[${_csrf.token}]]*/ null;
			var csrfHeader = /*[[${_csrf.headerName}]]*/ null;
			$(document).ajaxSend(function (e, xhr, options) {
				xhr.setRequestHeader(csrfHeader, csrfToken);
			});
		});
	</script>
	<script type="application/javascript">
		$(function () {
			function tagRequest(url, tagTitle) {
				$.ajax({
					dataType: "json",
					autocomplete: {
						enabled: true,
						rightKey: true,
					},
					contentType: "application/json; charset=utf-8",
					method: "POST",
					url: "/settings/tags" + url,
					data: JSON.stringify({'tagTitle': tagTitle})
				}).done(function (data, status) {
					console.log("${data} and status is ${status}");
				});
			}

			function onAdd(e) {
				tagRequest("/add", e.detail.data.value);
			}

			function onRemove(e) {
				tagRequest("/remove", e.detail.data.value);
			}

			var input = document.querySelector('input[name=tags-outside]');

			var tagify = new Tagify(input, {
				pattern: /^.{0,20}$/,
				dropdown: {
					position: "input",
					enabled: 0 // suggest tags after a single character input
				} // map tags
			});

			tagify.on("add", onAdd);
			tagify.on("remove", onRemove);
		});
	</script>
</body>

</html>
```

fragments.html
```html
<style>
	///
	.tagify--outside {
		border: 0;			

	}

	.tagify--outside .tagify__input {
		order: -1;
		flex: 100%;
		border: 1px solid var(--tags-border-color);
		margin-bottom: 1em;
		transition: .1s;
	}

	.tagify--outside .tagify__input:hover {
		border-color: var(--tags-hover-border-color);
	}

	.tagify--outside.tagify--focus .tagify__input {
		transition: 0s;
		border-color: var(--tags-focus-border-color);
	}
</style>
```
