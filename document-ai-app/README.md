# document-ai-app-sdk

## STEPS TO CREATE APP:

STEP 1: SET UP

* Intall the AI Appstore.
* Clone or download the sample directory "ai-apps-sdk" into your system, rename the folder "document-ai-app" to your desired name for the app.

STEP 2: SET UP THE APP ICONS

* Inside the "document-ai-app/app/images/intro" folder, create images which are named as Picture1.png, Picture2.png...etc, the images should have information of the app.
* Inside the "document-ai-app/listing/images/" folder, change the icon.png to your respective application icon.
* Similarily inside the "document-ai-app/app/images/" folder, change the icon.png to your respective application icon.

These simple steps will set your icons to the application you are creating.

STEP 3: CREATE THE DATA DICTIONARY

This is the critical step in creating your app.
* Edit the file "document-ai-app/app/data_dictionary.yml" in accordance with the data that your documents contain.

For Example: 

Let's say you are creating an app to train AI on your employee information documents. You will have to create the data_dictionary.yml accordingly, firstly by classifying your data into groups, then mapping the fields (key-value pairs) to these groups inside the yaml file. 

In this example, we may classify the data into two groups, Employer Information and Employee Information. Refer to step3.png

The keys in this yaml are to be short and relevant, here they are "EMPRINFO" and "EMPINFO" for the groups
The mappings to the these groups will include the "DISPLAY_SHORT_NAME", which is the short display name of the data group, that will show on the UI and the "FIELDS", under which there is list of data fields. 

```
sample data_dictionary.yml:
----------------------------
VERSION: 2

EMPRINFO:
  DISPLAY_SHORT_NAME: Employer Information
  FIELDS:
    EMIN:
      DISPLAY_SHORT_NAME: Employer Identification Number
      VAL_GENERATOR: 
        FUNC: Number
      TYPE: Number
      IS_PREVIEW: true
    EMRN:
      DISPLAY_SHORT_NAME: Employer Name
      VAL_GENERATOR:
        FUNC: CompanyName
      TYPE: STRING
      IS_PREVIEW: true
    ...
EMPINFO:
  DISPLAY_SHORT_NAME: Employee Information
  FIELDS:
    EEIN:
      DISPLAY_SHORT_NAME: Employee Identification Number
      VAL_GENERATOR:
        FUNC: Number
      TYPE: Number
      IS_PREVIEW: true 
    EEFN:
      DISPLAY_SHORT_NAME: Employee First Name
      VAL_GENERATOR:
        FUNC: FName
      TYPE: STRING
      IS_PREVIEW: true
    EEAD:
      DISPLAY_SHORT_NAME: Employee Address
      VAL_GENERATOR:
        FUNC: Address
      TYPE: STRING
      MAX_PARTS: 2  
    ...
```
The each field node mapped to "FIELDS" has the following structure as shown above.

* DISPLAY_SHORT_NAME : For the display name on UI
* VAL_GENERATOR : The "FUNC" mapped to this, should contain the name of the field inside DataGenerator.generate() method of the data_generator.py
* TYPE : The datatype of the field
* MAX_PARTS : is used when there are fields in the document which are to be numerically labeled as field1, field2, field3.. etc.,. The number in the value of MAX_PARTS will generate that many fields with the field key name and the incremented numerical.
* IS_PREVIEW: If True, this field will be displayed on the App's table data, when the app starts.

STEP 4: MODIFY THE DATA GENERATOR

The "document-ai-app/app/data_generator.py" can be modified when required. The DataGenerator.generate() method generates data for the fields in the document. This is important to define what kind data needs to be generated for the fields.

For example:

For the field "EEAD" i.e, Employee Address, the data generated in the place of this field for training dataset has to be similar to addresses possible, hence we use the Faker API to creates such fields.
if there is a specific data type you will have to generate for a field, modify the "document-ai-app/app/data_generator.py" file:

```
def generate(self, field, row_no=None, part_no=None):
    ret_value = None
    if field == "Address_1":
        ret_value = self.aFake.address().split('\n', 1)[0]
    elif field == "Address_2":
        ret_value = self.aFake.address().split('\n', 1)[1]
    elif #add your field name here and logic to generate the data
    # this field name should match to the value for 'FUNC' mapped to 'VAL_GENERATOR'

```

STEP 6: EDIT THE HELPTEXT-EN.YAML, APP.YAML & DOCUMENT_APP.YAML

* Rename the file "document-ai-app/app/document-ai-app.exe" after your app name
* Edit the values for the given keys in "document-ai-app/app/helptext-en.yaml", "document-ai-app/app/document_app.yaml" and "document-ai-app/listing/app.yaml".
* For the fields "LAUNCH_COMMAND" in "document-ai-app/app/document_app.yaml" and "document-ai-app/listing/app.yaml" give the renamed exe file.


STEP 5: MAKE THE ZIP

* Create a zip file once the files are modified. The zip needs to contain the folders 'app','listing' and Manifest.yaml.
* Name the zip file as : "your_App_Name".aiapp, keep extension ".aiapp"

STEP 6: TEST THE APP

* Launch AI Appstore.
* To test if the new app you created is working, create a folder "apps.local" inside "%LocalAppData%\DeepCognition\ai-app-store\temp-cache" (to navigate to this path, press 'WINDOWS'+'R' and when the run console shows up, paste this path there, press 'ENTER') , and place the zip file there. This folder will be created only if the AI AppStore app is installed and launched.
* Now you will find your app in the AI Appstore UI, under the Available Apps section.
* Install and launch your application to test.


STEP 6: EMAIL

* Email us at hello@deepcognitiona.ai, the zip file once the zip file is created. We will validate the files and provide you with the release of your app on the AI Appstore.
