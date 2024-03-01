<!-- Steps to build apk from aab file -->


# Converting AAB to APK using Bundletool

## Step 1: Install Bundletool
- First, you need to download and install Bundletool. Bundletool is a command line tool that you can use to create an Android App Bundle and convert it into APKs.

    ```bash
    # Download the latest bundletool jar file
    wget https://github.com/google/bundletool/releases/latest/download/bundletool-all-latest.jar
    ```
- Or you can manually download by visiting following url and click on latest available version of .jar file:
    [https://github.com/google/bundletool/releases](https://github.com/google/bundletool/releases)

## Step 2: Generate APKs from AAB
Use the following command to generate APKs from your AAB file:

```bash
# You will need Java to be installed to issue this command. If you don't have java installed, install it first and then execute following command
# If your bundletool jar file and aab file are not in same directory / folder then 
# Replace 'my_app.aab' with the name of your .aab file
# Replace 'bundletool.jar' with the name of your bundletool file
# Replace 'output' with the directory where you want to save the generated APKs
java -jar bundletool.jar build-apks --bundle=my_aab.aab --output=output.apks --mode=universal
```

## Step 3: Extract APK files from the APKS
The above command generates an .apks file, which is a ZIP file containing the individual APK files.

- You can extract it using any zip extraction tool.
    ```bash
    # Replace 'output.apks' with the path to your .apks file
    unzip output.apks
    ```
- Or you can rename and change the file extension of 'output.apks' from **.apks to .zip** and then extract it.
  
**You should now have your APK files extracted and ready for use.**


Please replace `'my_app.aab'` and `'output'` with your actual AAB file path and desired output directory respectively. Also, ensure that you have Java installed on your system as it's required to run the `bundletool` jar file.

