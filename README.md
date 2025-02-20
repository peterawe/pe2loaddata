# pe2loaddata
Script to parse a Phenix metadata XML file and generate a .CSV for CellProfiler's loaddata module

To install: 

```
git clone https://github.com/broadinstitute/pe2loaddata.git
cd pe2loaddata/
pip install -e .
```

To run CSV creation based on the XML file:

    pe2loaddata --index-directory <index-directory> config.yml output.csv

where <index-directory> is the directory containing the Index.idx.xml file and the images (any image set that is not complete will not be written to the CSV), config.yml is the LoadData configuration file and output.csv is the CSV that will be generated.

The config.yml file lets you name the channels you want to save and lets you pull metadata out of the image. An example:

    channels:
        HOECHST 33342: OrigDNA
        Alexa 568: OrigAGP
        Alexa 647: OrigMito
        Alexa 488: OrigER
        488 long: OrigRNA
    metadata:
        Row: Row
        Col: Col
        FieldID: FieldID
        PlaneID: PlaneID
        ChannelID: ChannelID
        ChannelName: ChannelName
        ImageResolutionX: ImageResolutionX
        ImageResolutionY: ImageResolutionY
        ImageSizeX: ImageSizeX
        ImageSizeY: ImageSizeY
        BinningX: BinningX
        BinningY: BinningY
        MaxIntensity: MaxIntensity
        PositionX: PositionX
        PositionY: PositionY
        PositionZ: PositionZ
        AbsPositionZ: AbsPositionZ
        AbsTime: AbsTime
        MainExcitationWavelength: MainExcitationWavelength
        MainEmissionWavelength: MainEmissionWavelength
        ObjectiveMagnification: ObjectiveMagnification
        ObjectiveNA: ObjectiveNA
        ExposureTime: ExposureTime

In the above example, "HOECHST 33342" is the label for the DNA channel and
if you load the .csv file in LoadData, you will get an image named "DNA" in
your image set.

The metadata section selects items out of the image metadata and allows
you to rename them as metadata. In addition, pe2loaddata automatically
populates the plate, well and site metadata entriess.

pe2loaddata now supports experiments with multiple planes per field as long as the `PlaneID` field 
has been set in the config file.

------

To run CSV creation based on the XML file, AND to append illumination columns (note that this requires 
the CellProfiler names in your config file to start with `Orig`, which will be replaced by `Illum`)

    pe2loaddata --index-directory <index-directory> config.yml output.csv --illum --illum-directory <illum-directory> --plate-id <plate-id> --illum-output output_with_illum.csv

where <illum-directory> is the directory where illumination files are (or will be) and <plate-id> is the plate ID that will be used by CellProfiler in your illumination files' names.
    
If you've already generated `output.csv` and want to only add the illum files to it, you can run with 

    pe2loaddata --index-directory <index-directory> config.yml output.csv --illum-only --illum-directory <illum-directory> --plate-id <plate-id> --illum-output output_with_illum.csv
