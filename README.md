# Ostorlab Github Action
![Ostorlab ci_can for GithubAction](https://i.ibb.co/XF3cwWw/image.png)

Ostorlab scans mobile applications (Android APK, AAB, iOS IPA) for security and privacy issues. It provides full
coverage by detecting issues on both the client-side and service-side, covering both the application code and
all of its dependencies.

Ostorlab supports all major frameworks, both native and multi-platform. This includes Java, Kotlin, Objective C,
Swift, Flutter, Cordova, React Native, Ionic and Xamarin.

Ostorlab provides both static and dynamic analysis capabilities, detecting over 500 vulnerability classes, like
hardcoded secrets, privacy data leakage, untrusted input inject, outdated dependencies with a database of over 120k
known vulnerable dependency. 

## Getting Started

To use Ostorlab Github Action, the first step is to generate an API key. To do so, simply follow the following steps:

1. Go to the [API keys menu](https://report.ostorlab.co/library/api/)
2. Click the new button to generate a new key
3. Copy the api key (You can add a name and an expiry date to your key)
4. Click the save button to save your key
 (You can add a name and an expiry date to your key), do not forget to click the save button to save your key.

![api key](https://github.com/jenkinsci/ostorlab-plugin/raw/master/images/jenkins-apikey.png)

Once you have generated your API, add it to GitHub Secrets. Make sure the name matches the secrets.<name> in the YAML
file. You follow these steps for more detailed instructions [Github:Creating encrypted secrets for a repository](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository)

The next steps is to a update your workflow to add an Ostorlab step to trigger the scan. Below is a sample performing
a rapid scan on an Android APK and failing the pipeline on vulnerabilities with `HIGH` severity.
    
```yaml   
on: [push]  
jobs:
   ostorlab_test:
   	runs-on: ubuntu-latest
   	name: Test ostorlab ci actions.
   	steps:
   	 - uses: actions/checkout@v2
   	 - name: Lunch Ostorlab scan
   	   id: start_scan
   	   uses: actions/ostorlab_actions@v1
   		with:
   		 plan: rapid_static 
   		 asset_type: android-apk 
   		 target: andoird_apk.apk
   		 can_title: title_scan_ci
   		 ostorlab_api_key: ${{ secrets.ostorlab_api_key }} # your secret api key.
   		 break_on_risk_rating: HIGH 
   		 max_wait_minutes: 20 
```   
    
### Action inputs 

The Github actions the following options:
    
 - **`plan`** *(['rapid_static', 'static_dynamic_backend'])*: [Required] - Specifies the scan plan ( `rapid_static` for fast static only analysis and `full analysis` for full static, dynamic and backend coverage).
- **`asset_type`** *(['android-apk', 'android-aab', 'ios-ipa'])*: [Required] - Target asset, Ostorlab supports APK, AAB and IPA.    
- **`target`**: [Required] - target file to scan.    
- **`ostorlab_api_key`**: [Required] -  API Key from Ostorlab portal.     
- **`scan_title`**: [Optional] - A scan title to identify your scan.
- **`break_on_risk_rating`** *(['HIGH', 'MEDIUM', 'LOW','POTENTIALLY])*: [Optional] -  Wait for the scan results and force the action to fail if the risk rating match or is higher than the provided value.    
- **`max_wait_minutes`**: [Optional] - Max wait time in minutes, pipeline will not fail if the scan times out.

### More details   
- [ostorlab.co](https://www.ostorlab.co/)  
- [Risk Ratings](https://docs.ostorlab.co/guide/#risk-ratings)  
- [Ostorlab SDK](https://github.com/ostorlab/ostorlab)
