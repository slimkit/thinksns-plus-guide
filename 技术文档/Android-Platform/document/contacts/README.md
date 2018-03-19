2017年10月17日09:44:11

# 联系人说明
此库引用地址：https://github.com/tamir7/Contacts


### 使用说明
Initialize Contacts
```java
public class MyApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        Contacts.initialize(this);
    }
```
Get All Contacts

```java
List<Contact> contacts = Contacts.getQuery().find();
```

Get Contacts with phone numbers only

```java
Query q = Contacts.getQuery();
q.hasPhoneNumber();
List<Contact> contacts = q.find();
```

Get Specific fields

```java
Query q = Contacts.getQuery();
q.include(Contact.Field.DisplayName, Contact.Field.Email, Contact.Field.PhotoUri);
List<Contact> contacts = q.find();
```

Search By Display Name

```java
Query q = Contacts.getQuery();
q.whereContains(Contact.Field.DisplayName, "some string");
List<Contact> contacts = q.find();
```

Find all numbers with a specific E164 code

```java
Query q = Contacts.getQuery();
q.whereStartsWith(Contact.Field.PhoneNormalizedNumber, "+972");
List<Contact> contacts = q.find();
```

Find a Contact by phone Number

```java
Query q = Contacts.getQuery();
q.whereEqualTo(Contact.Field.PhoneNumber, "Some phone Number");
List<Contact> contacts = q.find();
```

Get all Contacts that their name begins with a specific string OR their phone begings with a specific prefix.
```java
Query mainQuery = Contacts.getQuery();
Query q1 = Contacts.getQuery();
q1.whereStartsWith(Contact.Field.DisplayName, "Some String");
Query q2 = Contacts.getQuery();
q2.whereStartsWith(Contact.Field.PhoneNormalizedNumber, "+972");
List<Query> qs = new ArrayList<>();
qs.add(q1);
qs.add(q2);
mainQuery.or(qs);
List<Contact> contacts = mainQuery.find();

```
### 修改说明

给 bean 增加了序列化实现