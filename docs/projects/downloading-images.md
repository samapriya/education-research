# **Downloading Images**
You can download images from Planet using a couple of methods, including but not limited to Planet Explorer or using a client to make requests to the Data API and downloading imagery.

### **Planet Explorer**
Planet Explorer is probably one of the most useful and beloved interface to interact with and download Planet Labs satellite imagery. Not only does it allow you to filter your images to specific sensors but it also allows you to filter by cloud cover among other things. A neat little trick in Planet Explorer is that once the images are filtered if you want to download multiple images at once in an order you can hold down the control key(if using a windows machine) and click on multiple sets of imagery adding them to the same order.

![Using the Planet Explorer](../images/downloading_images.gif)

<center>_Steps to get satellite imagery from Planet Explorer_</center>

Once the images have been ordered sit back and relax as the order notification that your delivery is ready to be picked up will be emailed to you.

To interact with the **Data API** and batch download imagery there is [host of Planet Platform documentation](https://www.planet.com/docs/) that teach you how to do that step by step.

### **Batch Download Images**

#### Using Planet Python CLI
This is the default command line tool that is provided by planet and you can [find the project here](https://github.com/planetlabs/planet-client-python) and it connects to Data API to perform multiple operations such as quick search, activate and download assets among a few other things. Install it easily using

 <center>**```pip install planet```**</center>

 follow it by ```planet init``` to initialize and you are ready to go.

 A simple setup to download images using a geometry(a simple geometry geojson file) and date range (let us say between 2018-07-01 to 2018-08-31), cloud cover(less than 10% or less than 0.1)  could be achieved in a single line of cli command


```planet data download --item-type PSScene4Band --geom "full path to geometry.geojson" --date acquired gt 2018-07-01 --date acquired lt 2018-08-31 --range cloud_cover lt 0.1 --asset-type analytic --dest "your local directory path"```

#### Batch Download and Upload to Earth Engine
I have also created another stand alone tool apart from the [ppipe tool](https://pypi.org/project/ppipe/) to just download imagery once you have selected area of interest and sensor type. This will work if you have the (Planet Python Client installed). Incase you missed this in housekeeping you can [read it again](https://samapriya.github.io/open-impact/terra2018/projects/housekeeping/).

It is using the Saved search function to allow you to batch download. You can find the [tool here](https://github.com/samapriya/planet-standalone-tools). You can pip install it using

<center>**```pip install ppipe```**</center>

This tool is a quick addon to existing application of planet saved searches to download images. This prints all the saved searches that you might have saved using the CLI or using the explorer. In which case you are able to set the filters, choose item types and date ranges and aoi within the Planet Explorer GUI and then be able to use the saved search name to execute a batch download command. This combines activation and download and works only for a single item type that was set in the search. You can choose to provide a limit which limits the number of item-asset combinations to download or use without limit and all items and asset combinations in the aoi will be downloaded.

Using with limits

```
python saved_search_download.py "search_name" "analytic" "C:\planet_demo" "10"
```

Without limits the setup becomes

```
python saved_search_download.py "search_name" "analytic" "C:\planet_demo"
```

![Using the Saved Search Tool](../images/saved_searches.gif)

I added the functionality to use Planet's own async downlaoder to download based on a geometry

A setup using geojson needs to include other filters too and a typical setup would be

```ppipe dasync --infile "C:\Users\johndoe\geometry.geojson" --item "PSScene4Band" --asset "analytic" --local "C:\planet" --start "2018-06-01" --end "2018-08-01" --cmin 0 --cmax 0.4```

Using a stuctured json file that you might have created earlier means you don't have to pass additional filters everytime

```python ppipe.py dasync --infile "C:\Users\johndoe\geometry.json" --item "PSScene4Band" --asset "analytic_xml" --local "C:\planet_demo"```

However, you can still decide to pass the filters and the filters you pass will overwrite existing filters

```python ppipe.py dasync --infile "C:\Users\johndoe\geometry.json" --item "PSScene4Band" --asset "analytic_xml" --local "C:\planet_demo" --start "2018-06-01" --end "2018-08-01" --cmin "0" --cmax 0.4```
