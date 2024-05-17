1 Generate keystore
https://coderwall.com/p/r09hoq/android-generate-release-debug-keystores

To generate keystores for signing Android apps at the command line, use::
>keytool -genkey -v -keystore my-key.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000
A debug keystore which is used to sign an Android app during development needs a specific alias and password combination as dictated by Google. To create a debug keystore, use:
>keytool -genkey -v -keystore debug.keystore -storepass android -alias androiddebugkey -keypass android -keyalg RSA -keysize 2048 -validity 10000

2 Get key fingerprints
To hook your app up with services like Google APIs you'll need to print out each of your keys' fingerprints and give them to the services you're using. To do that, use:
>keytool -list -v -keystore [keystore path] -alias [alias-name] -storepass [storepass] -keypass [keypass] 

For your debug key that would look like:
$ keytool -list -v -keystore debug.keystore -alias androiddebugkey -storepass android -keypass android 
(exmaple)>keytool -list -v -keystore ./awesome-project.keystore -alias awesome-project_alias -storepass ? -keypass ?