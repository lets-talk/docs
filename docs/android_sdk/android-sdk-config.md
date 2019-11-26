# Let's Talk Android SDK

You can download the Android SDK [here](https://s3.amazonaws.com/prod.letsta.lk/mobile-sdk/android/0.6.0/v4(OpcionesParaDeshabilitar)-20190826T222732Z-001.zip).

Basic feature list:

 * Login with multiple accounts as user of several domains
 * Loading an existing conversation
 * Creating new conversations
 * Listing clients, agents, inquiries and groups


This SDK includes interface components to using a chat or showing lists to select destinations to chat with.

### Required dependencies:

Please add the following dependencies on your application first to ensure the SDK properly works.

```
implementation 'com.squareup.retrofit2:converter-gson:2.3.0'
implementation 'com.android.support:recyclerview-v7:27.1.1'
implementation 'com.android.support:design:27.1.1'
implementation 'com.pubnub:pubnub-gson:4.10.0'
implementation 'com.google.android.gms:play-services-maps:16.0.0'
implementation 'info.guardianproject.netcipher:netcipher:1.2'
implementation 'ru.noties:markwon:2.0.0'
implementation 'com.github.jkwiecien:EasyImage:2.1.0'
implementation 'android.arch.persistence.room:runtime:1.1.1'
annotationProcessor 'android.arch.persistence.room:compiler:1.1.1'
implementation 'android.arch.lifecycle:livedata:1.1.1'
```

And run as gradle build to resolve them :+1:

### Activities declaration:

The SDK provides interfaces for using the chat or listing possible destination to start a new one: Clients, Agents, Inquiries and Groups. Also, provides interfaces for sending location as message and needs other support activities. Please add them to your project's AndroidManifest.xml file

```
        <activity android:name="com.ltmessenger.letstalksdk.activities.SendLocationActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.AddMemberToConversationActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.ConversationDetailsActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.InquiriesActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.AgentsActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.ClientsActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.GroupsActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.AddGroupActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.AgentsSelectionActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.WebLoginActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.ConversationActivity"
            android:screenOrientation="portrait"/>

        <activity android:name="com.ltmessenger.letstalksdk.activities.ConversationsActivity"
            android:screenOrientation="portrait" />

        <activity android:name="com.ltmessenger.letstalksdk.activities.WebViewActivity"
            android:screenOrientation="portrait"
            android:theme="@style/Theme.AppCompat.Light.NoActionBar"/>
```

### Providers declaration:
After the usage of API 25 (Lollipop) the following declaration is needed for opening received files by chat :
```
        <provider
            android:name=".provider.LTProvider"
            android:authorities="${applicationId}.provider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/provider_paths"/>
        </provider>
```

### Google Maps configuration:

To ensure correct working of the Google Maps usages that Let's Talk SDK needs, you have to complete some steps first.

* Register your new application on [Google API Console](https://console.developers.google.com/)
* Enable the Google Maps API and get an API Key
* Declare the API Key on AndroidManifest.xml file

```
<meta-data android:name="com.google.android.geo.API_KEY" android:value="<YOUR_API_KEY>"/>
```

## Usage

To get the SDK started to use, please add the following line inside the `onCreate` method of your Application class:

```
LetsTalkSDK.init(this);
```

### 1) Login:

`LetsTalkSessionManager` is the class used to manage the login and logout of new users

```
        LetsTalkSessionManager sessionManager = new LetsTalkSessionManager.Builder()
                .withBaseURL("https://yourcompanydomain.letsta.lk")
                .withUsername("the_username")
                .withPassword("the_password")
                .build();

        sessionManager.connect(this, this.connectionListener);
```

`ConnectionListener` is the class that manages the callback of the operation result

```
private ConnectionListener connectionListener = new ConnectionListener() {

        @Override
        public void success() {
            //Successfully connection
        }

        @Override
        public void failure(Throwable t) {
            //Failed connection (See log on LogCat for more information)
        }
    };
```

### 2) Listing selectable entities:

Let's Talk SDK offers the following possible destinations for starting a chat:

* Inquiries
* Agent
* Group
* Client

The way to start the Activity that lists them is as the example below:

```
startActivityForResult(new Intent(this, InquiriesActivity.class), REQUEST_CODE_SELECT_INQUIRY);
```

That example will open a new Activity that lists Inquiries and then return as a result the selected one, so you can manage the selection and the do an specific action, as beginning a new chat.

These are the activities that returns a selected entity and their respectives types of return value:

* `InquiriesActivity` -> `Inquiry`
* `AgentsActivity`-> `Person`
* `ClientsActivity` -> `Client`
* `GroupsActivity` -> `Group`

For example, to get the selected inquiry on our example, we can do:

```
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if (requestCode == REQUEST_CODE_SELECT_INQUIRY && resultCode == RESULT_OK) {
            Inquiry selected = (Inquiry) data.getSerializableExtra(LTConstants.EXTRA_INQUIRY);
            Toast.makeText(this, "Selected: " + selected, Toast.LENGTH_SHORT).show();
        }
    }
```
The constants values that can be used to get the serializable extras are:

* `LTConstants.EXTRA_INQUIRY`
* `LTConstants.EXTRA_PERSON`
* `LTConstants.EXTRA_CLIENT`
* `LTConstants.EXTRA_GROUP`

Also, you can pass an EXTRA parameter to make the Activity not returning the selected value and just open a chat with the selected entity. The way to build the Intent and achieve it is like following: 

```
        Intent intent = new Intent(this, ClientsActivity.class);
        intent.putExtra(LTConstants.OPEN_CHAT_ON_SELECT, true);
```

### 3) Creating new conversations:

As said previously, you can get the selectable item chosen and use it to start a new conversation. 
The way to do it is as the following complete example:

```
        LetsTalkConversationLoader.getInstance().loadWith(
                new ConversationDescription.Builder(this, this.conversationCreated)
                        .withInquiry(selectedInquiry)
                        .setInternal(false)
                        .includeMe(true)
                        .build());
```

#### NOTES
* In the example above we are starting a new conversation with an inquiry, but if you want to start it with other selectable entity, the way is analog. You can use, by example: `.withGroup(selectedGroup)` instead of `.withInquiry(selectedInquiry)`
* You can pass directly the selectable entity or, if you only know the id, is possible to do, for example: `.withInquiryId(23L)`
* By default the conversation is set as `internal = false`
* By default the conversation is set as `includeMe = true`

You must pass an implementation of `ConversationDescription.ConversationCreatedCallback` as a mandatory parameter to handle the result of creating the conversation. An example implementation is provided below:

```
  private ConversationDescription.ConversationCreatedCallback conversationCreated = new ConversationDescription.ConversationCreatedCallback() {
        @Override
        public void success(Conversation conversation) {
            //Manage the created conversation that you receive on this method
        }

        @Override
        public void error(Throwable t) {
            //Manage the creation error
        }
    };
```
### 4) Loading an existing conversation:

You can also load a conversation that you know the id in the following way:

```
LetsTalkConversationLoader.getInstance().loadExisting(this, conversationId, this.conversationLoaded);
```

You must pass an implementation of `LetsTalkConversationLoader.ConversationLoadedCallback` as following example:

```
  private LetsTalkConversationLoader.ConversationLoadedCallback conversationLoaded = new LetsTalkConversationLoader.ConversationLoadedCallback() {
        @Override
        public void loadSucceded(Conversation conversation) {
            //Manage the loaded conversation
        }

        @Override
        public void loadingError(Throwable throwable) {
            //Manage the error loading the conversation
        }
    };
```

### 5) Using the chat interface:

The chat interface is actually provided as a Fragment of the class `ConversationFragment`
You can include it on your Activities as they inherit from FragmentActivity.
Instantiate them with the conversation object that you want to load and add it to the Activity.
An example is provided below:

```
  ConversationFragment conversationFragment = ConversationFragment.newInstance(conversation);
  FragmentTransaction fragmentTransaction = getSupportFragmentManager().beginTransaction();
  fragmentTransaction.replace(R.id.fragmentPlaceholder, conversationFragment).commitAllowingStateLoss();
```
