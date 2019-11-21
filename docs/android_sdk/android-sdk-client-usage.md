# Client app usage

The SDK can be used in similar way as previous to build a client app. Some classes changes but it works analog to explained above.

To get the SDK started to use, please add the following line inside the `onCreate` method of your Application class:

```
LetsTalkSDK.init(this);
```

### 1) Account management

The main class to manage a client account is `LetsTalkClientAccountManager`
There are several options to do a login in a client app, described as following:

#### Guest login

```
        new LetsTalkClientAccountManager.Builder()
                .asGuestLogin()
                .withSubdomain("domain")
                .withIdentifier(<IDENTIFIER>)
                .withEmail(<EMAIL>)
                .build().connect(this, this.connectedListener);
```

The result is received on a callback.

#### Login using a custom webview

```
        new LetsTalkClientAccountManager.Builder()
                .asWebLogin()
                .withSubdomain("domain")
                .withRedirectSchema(<SCHEMA>)
                .startForResult(this, WEB_LOGIN_REQUEST_CODE);
```

The result is received through `onActivityResult` overriding. 

#### Login by token and provider

```
        new LetsTalkClientAccountManager.ByTokenAndProviderBuilder(<TOKEN>, <PROVIDER>)
                .build()
                .connect(this, this.connectedListener);
```

#### Login by sponsored client method and extra attributes

```
        Map<String, String> attrs = new HashMap<>();
        attrs.put("Puesto", "Supervisor");
        attrs.put("Legajo", "1322");
        new LetsTalkClientAccountManager.Builder()
                .asSponsoredClient()
                .withSubdomain("domain")
                .withKey(<KEY>)
                .withToken(<TOKEN>)
                .withName(<NAME>)
                .withUID(<UID_TYPE>, <UID_RUT_OR_EMAIL>)
                .withAttrs(attrs)
                .build()
                .connect(this, this.connectionListener);
```

Where `UID_TYPE` can be `UIDType.RUT` or `UIDType.EMAIL` and UID value according to this type.

#### Get the current logged user

```
        Account currentAccount = LetsTalkClientAccountManager.getCurrentAccount(this);
```

#### Disconnect current logged user

```
        LetsTalkClientAccountManager.disconnectCurrentAccount(this);
```

#### Update client avatar

```
        File avatarFile = new File(<FILE_PATH>);
        
        LetsTalkClientAccountManager.changeProfilePhoto(WLClientActivity.this, avatarFile, 
                new ProfileUpdatedListener() {
                        @Override
                        public void profileUpdated() {
                            Log.i(TAG, "profile photo updated" );
                        }

                        @Override
                        public void error() {
                            Log.w(TAG, "error updating profile photo");
                        }
                    });
```

### 2) Load inquiries

The result is received through a callback

```
        LetsTalkClientChatManager
                .getInstance()
                .loadInquiries(this, this.inquiriesLoaded);
```

Other way is fetching the inquiries and use the default provided views to show them and manage the click on select inquiries to start conversations, open a URL or show sub-inquiries.

```
        Intent intent = new Intent(this, InquiriesActivity.class);
        intent.putExtra(LTConstants.OPEN_CHAT_ON_SELECT, true);
        startActivityForResult(intent, 1);
```

If `intent.putExtra(LTConstants.OPEN_CHAT_ON_SELECT, true);` is not passed or is `false`, the result of selection is managed onActivityResult, else it's auto managed by the SDK.

If the organization has no inquiries, a conversation can be created anyway. In this case, you can init a new conversation as following:

```
        LetsTalkConversationLoader.getInstance()
                .loadWith(new ConversationDescription.Builder(this, null)
                .withNoInquiry()
                .build());
```

A new conversation activity is opened and ready to chat.

### 3) Load conversations

The result is received through a callback

```
        LetsTalkClientChatManager
                .getInstance()
                .loadConversations(this, this.conversationsLoaded);
```

Other way is fetching the conversations and use the default provided views to show them. 

```
        LetsTalkClientChatManager
                .getInstance()
                .showConversations(this, ConversationState.ALL);
```

If you want to use a view that shows conversations and also allows to start a new one from that screen, you can use the following method:

```
        LetsTalkClientChatManager
                .getInstance()
                .showConversationsWithNewOption(this, ConversationState.ALL);
```
