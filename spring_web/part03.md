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

# 36.  관심 주제 등룍 뷰
* npm install @yaireo/tagify
