# AutoFreddyEncodable/AutoFreddyDecodable

Handles `JSONEncodable` and `JSONDecodable`  protocols automatically based on your struct or class. Encode and decode is split into 2 annotations for ease of implementation

## Notes
This implementation is in it's basic form, handling basic types. There are obviously more complex JSON data structures that this currently does not handle (ie `enum`)

We also handle date through a `JSONDateWrapper` which will convert dates from JSON

## Options

### PropertyName
- Allows you to change what the property encodes/decodes from the originating json

### NoMock
- Skips a property to mock

### JSONRoot
- On Decode - starts looking at the root
- On Encode - wraps dictionary

## Example

```
// sourcery: AutoFreddyEncodable
// sourcery: AutoFreddyDecodable
// sourcery: JSONRoot=response
public struct AuthorizationResponse: UserTokens {

    // sourcery: PropertyName=refresh_token
    public var refreshToken: String
    // sourcery: PropertyName=token_type
    public var tokenType: String
    // sourcery: PropertyName=access_token
    public var accessToken: String
    // sourcery: PropertyName=expires_in
    public var expiresIn: Int

}
```

```
extension AuthorizationResponse: JSONEncodable {
    public func toJSON() -> JSON {
        var jsonDictionary: [String: JSON] = [:]
        jsonDictionary["refresh_token"] = .string(refreshToken)

        jsonDictionary["token_type"] = .string(tokenType)

        jsonDictionary["access_token"] = .string(accessToken)

        jsonDictionary["expires_in"] = .int(expiresIn)

        let wrapper: [String: JSON] = ["response": .dictionary(jsonDictionary)]
        return .dictionary(wrapper)
    }
}

extension AuthorizationResponse: JSONDecodable {
    public init(json: JSON) throws {
        let jsonRoot = JSON("response")
        refreshToken = try jsonRoot.getString(at: "refresh_token")
        tokenType = try jsonRoot.getString(at: "token_type")
        accessToken = try jsonRoot.getString(at: "access_token")
        expiresIn = try jsonRoot.getInt(at: "expires_in")
    }
}
```
