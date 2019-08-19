# User Setting

1. set client user for login / socoial network login

```objective-c
[[AotterTrek sharedAPI] setCurrentUserWithEmail:<user_email>
    phone:<user_phone>
    fbId:<user_fbId>
    gender:<user_gender>
    birthday:<user_birthday>
    addtionalMeta:<meta>
];
```

1. update userObject for more info updated

```objective-c
[[AotterTrek sharedAPI] updateCurrentUserWithValue:@"deviceId" 
                                     forKey:ATUserKeyDeviceID];
```

#### enum: ATUserKey

| enum name         | value type | description  |
| ----------------- | ---------- | ------------ |
| TKUserKeyAdId     | NSString   |              |
| TKUserKeyEmail    | NSString   |              |
| TKUserKeyPhone    | NSString   |              |
| TKUserKeyFbId     | NSString   |              |
| TKUserKeyGender   | BOOL       | male for YES |
| TKUserKeyBirthday | String     | yyyy/MM/dd   |

1. remove user when the client user logout.

```objective-c
  [[AotterTrek sharedAPI] removeCurrentUser];
```