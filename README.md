# Checkout-Android-Package

Checkout SDK for Blusalt

Get your API credentials from [Blusalt](https://blusalt.net/)

## Features

1. Received Money Via Card
2. Received Money Via Bank Transfer

   
## Installation

#### Step 1

Create a [github.properties] file in root of android folder and put below into content. E.g. "myDemoApp/android/github.properties"
Replace values with your github credentials from github and make sure to grant necessary permissions especially for github packages

```
USERNAME_GITHUB=SampleUsername
TOKEN_GITHUB=SampleClassicToken
```

#### Step 2

Add below to project level gradle file `/android/build.gradle`

```gradle
buildscript {
    ext.kotlin_version = '1.9.+'
    ...

    dependencies {
        classpath 'com.android.tools.build:gradle:7.3.+'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}

allprojects {
    def githubPropertiesFile = rootProject.file("github.properties")
    def githubProperties = new Properties()
    githubProperties.load(new FileInputStream(githubPropertiesFile))


    repositories {

        maven {
            name "GitHubPackages"
            url 'https://maven.pkg.github.com/Blusalt-FS/Checkout-Android-Package'

            credentials {
                username githubProperties['USERNAME_GITHUB']
                password githubProperties['TOKEN_GITHUB']
            }
        }
    }
}
```

#### Step 3

Change the minimum Android sdk version to 24 (or higher) in your `/android/app/build.gradle` file.

```

android {
    ...
    defaultConfig {
      ...
      minSdkVersion 24
    }
    ...
    
    ...
    dependencies {
      implementation 'net.blusalt:checkout:1.0-1' 
    }
    ...
}
```

#### Step 4

If you're experiencing crashes or timeouts.
Add below to your progaurd.pro file if using progaurd or minify is enabled  `/android/app/proguard-rules.pro`

```proguard
-keep class net.blusalt.checkout.** { *; }
```

Enable proguard in `/android/app/build.gradle` file like below.

```

android {
    
    ...
    buildTypes {
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
    ...
}
```

## USAGE

```sh
## To use SDK
String email = ""; (Optional, can be null)
String reference = ""; (Optional, can be null)
Object metadata = null; (Optional, can be null)


        BlusaltCheckout checkout = new BlusaltCheckout.BlusaltCheckoutBuilder(TestAc.this)
                .setApiKey("")
                .setIsPayment(false)
                .setIsDev(true)
                .setAmount(1000)
                .setCurrency("NGN")
                .setWalletId("master")
                .setUserEmail(email)
                .setReference(reference)
                .setMetaData(metadata)
                .setListener(new CheckoutCompletedCallBack() {
                    @Override
                    public void onSuccess(CheckoutSuccess response) {
                       
                    }

                    @Override
                    public void onFailure(int statusCode, String errorObject) {
                   
                     }
                }).build()

```

## Note:

If you are getting an error on android which says "Unauthorized" when gradle is downloading or
building, generate a new
github token that have access to clone, read and write to repo, access github packages. If you don't
know which to tick, tick all boxes. Cheers
