# 44. PostgreSQL

SQL Shell(psql)
```
Server [localhost]:
Database [postgres]:
Port [5432]:
Username [postgres]:
postgres 사용자의 암호:
psql (15.0)
도움말을 보려면 "help"를 입력하십시오.

postgres=#  create database testdb;
CREATE DATABASE
postgres=# create user testuser with encrypted password 'testpass';
CREATE ROLE
postgres=# grant all privileges on database testdb to testuser;
GRANT
postgres=#

```
* testuser role 부여  
postgres >  Roles >  testuser > View Role > Super User Check


* eclipse profile 설정  
Boot Dashboard > Open the launch configuration ..(연필 아이콘) > Spring Boot > Profile

# 47. EmailSErvice 추상화
# 48. Html Mail

AppProperties.java
```java
@Data
@Component
@ConfigurationProperties("app")
public class AppProperties {
	private String host;
}
```

AccountService.java
```java
public void sendSignUpConfirmEmail(Account newAccount) {
    // @formatter:off
    Context context = new Context();
    context.setVariable("link", "/check-email-token?token=" + newAccount.getEmailCheckToken() +
            "&email=" + newAccount.getEmail());
    context.setVariable("nickname", newAccount.getNickname());
    context.setVariable("linkName", "이메일 인증하기");
    context.setVariable("message", "스터디올래 서비스를 사용하려면 링크를 클릭하세요.");
    context.setVariable("host", appProperties.getHost());
    String message = templateEngine.process("mail/simple-link", context);

    EmailMessage emailMessage = EmailMessage.builder()
            .to(newAccount.getEmail())
            .subject("스터디올래, 회원 가입 인증")
            .message(message)
            .build();
    // @formatter:on
    emailService.sendEmail(emailMessage);
}

public void sendLoginLink(Account account) {
    // @formatter:off
    Context context = new Context();
    context.setVariable("link", "/login-by-email?token=" + account.getEmailCheckToken() +
            "&email=" + account.getEmail());
    context.setVariable("nickname", account.getNickname());
    context.setVariable("linkName", "스터디올래 로그인하기");
    context.setVariable("message", "로그인 하려면 아래 링크를 클릭하세요.");
    context.setVariable("host", appProperties.getHost());
    String message = templateEngine.process("mail/simple-link", context);

    EmailMessage emailMessage = EmailMessage.builder()
            .to(account.getEmail())
            .subject("스터디올래, 로그인 링크")
            .message(message)
            .build();
    emailService.sendEmail(emailMessage);
    // @formatter:on
}
```

HtmlEmailService.java
```java
@Slf4j
@Profile("dev")
@Component
@RequiredArgsConstructor
public class HtmlEmailService implements EmailService {

	private final JavaMailSender javaMailSender;

	@Override
	public void sendEmail(EmailMessage emailMessage) {
		MimeMessage mimeMessage = javaMailSender.createMimeMessage();
		try {
			MimeMessageHelper mimeMessageHelper = new MimeMessageHelper(mimeMessage, false, "UTF-8");
			mimeMessageHelper.setTo(emailMessage.getTo());
			mimeMessageHelper.setSubject(emailMessage.getSubject());
			mimeMessageHelper.setText(emailMessage.getMessage(), true);
			javaMailSender.send(mimeMessage);
			log.info("sent email: {}", emailMessage.getMessage());
		} catch (MessagingException e) {
			log.error("failed to send email", e);
		}
	}
}

```

simple-link.html
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>스터디올래</title>
</head>
<body>
    <div>
        <p>안녕하세요. <span th:text="${nickname}"></span>님</p>

        <h2 th:text="${message}">메시지</h2>

        <div>
            <a th:href="${host + link}" th:text="${linkName}">Link</a>
            <p>링크가 동작하지 않는 경우에는 아래 URL을 웹브라우저에 복사해서 붙여 넣으세요.</p>
            <small th:text="${host + link}"></small>
        </div>
    </div>
    <footer>
        <small>스터디올래&copy; 2020</small>
    </footer>
</body>
</html>
```

application.properties
```properties

app.host=http://localhost:8080

```
