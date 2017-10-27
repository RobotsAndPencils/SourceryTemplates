# AutoStubbable

Allows the ability to automatically create stubs for tests and mock network returns.

## Notes
This template uses Fakery as a dependency to create fake data.

## Options

### StubValue
 - Allows you to change what the stub value is

### NoMock
- Skips a property to mock


## Example

```
// sourcery: AutoStubbable
public struct KeychainUser {

    public let userName: String
    public let userID: Int

    // sourcery: StubValue=UserType.executive
    public userType: Int

    public let store: SecureLoginStore
    public let user: User
}
```

```
public extension KeychainUser {
  static func stub (
    userName: String = .random,
    userID: Int = Faker.shared.number.randomInt(),
    userType: Int = UserType.executive,
    user: User = User.stub()
   ) -> KeychainUser {
      return KeychainUser(
         userName: userName,
         userID: userID,
         userType: userType,
         user: user
      )
   }
}
```
