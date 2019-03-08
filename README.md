# ExtractPhotos
This ArcGIS Pro sample project uses a python script and script tool to extract and sort photo attachments from a hosted feature layer in ArcGIS Online. It also improves photo file names and field names. 

## Why is the tool useful?
1. It puts all the photos into a folder.
There is a github script that does this but it creates poor files names and loses a connection to the attributes. I used this code in the functions.
Source = https://support.esri.com/en/technical-article/000011912

2. It creates a table with all the collected attributes and a field that matches the photo filename.

3. The sorting is useful. You might want to sort the photos into subfolders by the 'Creator' field.

4. You can create a custom file name for the photos based off your attributes.

5. It adds geometry fields to your table in Decimal Degrees.

## What does the tool create?

IT DOES NOT CREATE  any new tables or featureclasses in your project. In fact, the only thing you have in your project
#is the script tool and the original geodatabase of points and attachments.

It adds the following fields to your attachment table:
   - X and Y decimal degrees fields to your table, called POINT_X and POINT_Y.
   - Copies of the unique ID fields called FeatureID and AttachID that are created by the tool.
     (In the source data these are used as objectID and could change if the data was copied.)
   - It can copies all the (default survey123) fields and other fields from the point collection featureclass into the attach table.
     (If you are using survey123 these fields are: Creator, Editor, CreationDate, EditDate)
   - It copies any additional custom fields you are using, such as Category and Description or Type or Name or anything else
   Note: you need to know the field names and type them all in (case sensitive)

It add the following fields to your point featureclass:
   - It adds a featureID field, which is the same as the objectID, this is a unique field
   - It adds POINT_X, POINT_Y, and POINT_Z fields (if there is a z)

It creates the following output files:
   - In your output folder it places all the photos that were collected.
   - It names the photos based off a field you pass in and prepends it to a unique base name.
   - It makes subfolders based on a field and then sorts the photos into these subfolders.

# One Time Setup Instructions

## Open the ArcGIS Pro sample project
Open the included sample project

## Examine previous run results
Look at the output folder and see how it sorted the photos.
Look at teh attachment table and the featureclass and see the new fields.

## Optional - Export your Hosted Feature Layer 
In your Portal or AGOL org, export your hosted feature layer with Attachments to a FGDB and download it to your local computer as a zip file and unzip it to a folder. Copy this folder into your project folder. YOu can also use the sample photos included with the project as a test. This is fictitous data showing some photos of 3D printed objects, in case you are curious.

## Optional - Create Script Tool In Pro 
In ArcGS Pro open the project. Use the included script tool, or use your own project and create a similar script tool.

## Optional - Create output folder for photos
Make a single windows folder in your project. I called mine 'out' but you can call it whatever you like.
This is where the photos will be initially be placed. The tool will also create subfolders in this folder
based on your sortfield and make additional copies of the photos in these subfolders when it runs.

## Clean out old data before initial run
The tool comes with a post run view so you can see the results. You first need to clean it up and also after anytime you run it.

-remove the contents of the output folder, but leave that folder there and empty

-Remove all the new fields added in the attachment table (all the fields after the data field, starting with AttachID all theway to PhotoName). KEEP THE DATA FIELD!!!)

-remove the FeatureID, POINT_X, POINT_Y, and POINT_Z fields in the MyPoints (point) fc

##  Finally Run the script tool from arcgis pro

PARAMS: (when entering in the script tool omit the quotes)

features =  featureclass, collected gdb featureclass you downloaded from AGOL/Portal

attachments =  table, collected attachment gdb table you downloaded from AGOL/Portal

outfolder = folder, 'output' folder you need to have this empty and alteady there. Phtotos go there

sortfield = field or string, the sort field, ie  'CATEGORY' or 'Creator' for example. This field must also be present below in the userifields
  
localkey = string, 'GlobalID'  the local field name in the features that joines to the same value in the attachment

foreignkey = string, 'REL_GLOBALID' the foreign key field name in the attachment table that matches the features id

userfields = existing fields to include from the featureclass, in this format 'CATEGORY;DESCRIPTION;CreationDate;Creator;EditDate;Editor;Picture'  

photoprefacefield =  the field name used ot preface the photo filname (ie 'Creator' field name) so the photos are then named Creator_FeatureID_AttachID.jpg for instance. This field must also be present below in the userifields

