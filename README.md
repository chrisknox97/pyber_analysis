# PyBer with Matplotlib

## Overview of Analysis

### To utilize ``groupby()`` functions  to create a data summary from Pyber’s ride-sharing data to highlight the total rides, total drivers, total fares, and average fare by ride and per driver based on city type. As well as to use ``Matplotlib`` charting capabilities to visually display the total weekly fares in three city types (rural, suburban, and urban) over a period spanning January 2019 until April 2019. 

## Pyber Ride-Sharing Results

### Calculating The Average Fares Per Rider & Per Driver

To calculate the average fares per rider and per driver type, we first had to tabulate our total rides, total drivers, and total fares (sorting by city type) using ``groupy()`` against our data frames. The scripts for these tabulations are shown below: 

	  Tabulate the Total Rides for Each City Type
	  rides_by_type = pyber_data_df.groupby([“type’]).count()[“ride_id”]

	  Tabulate the Total Drivers for Each City Type
	  drivers_by_type = city_data_df.groupby([“type’]).sum()[“driver_count”]

	  Tabulate the Total Amount of Fares for Each City Type
	  sum_fares_by_type = pyber_data_df.groupby([“type’]).sum)[“fare”]

The results of these tabulations found that the highest totals in each field were in urban city types, with the lowest being in rural areas. 

<p float="left">

<img src="https://github.com/chrisknox97/pyber_analysis/blob/main/Tables/Total%20Rides%20by%20City%20Type.png" width ="300" height="250">
<img src="https://github.com/chrisknox97/pyber_analysis/blob/main/Tables/Total%20Drivers%20by%20City%20Type.png" width ="310" height="250">
<img src="https://github.com/chrisknox97/pyber_analysis/blob/main/Tables/Total%20Fares%20by%20City%20Type.png" width ="300" height="250">
	
</p>

These new totals were then used to calculate the average fare per ride and per driver using the following script:

	  Calculate the Average Fare by Ride
	  avg_fare_by_type = pyber_data_df.groupby([“type”]}.mean()[“fare”]

	  Calculate the Average Fare by Driver
	  avg_fare_per_driver = sum_fares_by_type / drivers_by_type
	  
<p float="left">
	
<img src="https://github.com/chrisknox97/pyber_analysis/blob/main/Tables/Average%20Fare%20by%20Ride.png" width ="300" height="250">
<img src="https://github.com/chrisknox97/pyber_analysis/blob/main/Tables/Average%20Fare%20by%20Driver.png" width ="300" height="250">

</p>

These calculations found that while rural areas had the lowest total rides, drivers, and total fares; the decreased competition and presumably further distances between destinations contributed to the highest average fares per driver and ride of all the city types. 

![Pyber Graph](https://github.com/chrisknox97/pyber_analysis/blob/main/Tables/Deliverable%201.png)

### Total Weekly Fares

We were also concerned about the total weekly fares by city type from January 2019 until April 2019. First we had to use ``groupby()`` to sort for weekly fares by city type and date, creating a new data frame the limited our data to the aforementioned essentials. 

	  Create a New DataFrame Showing Sum of Fares and Indexing for City Type & Date
	  sum_fares_by_type = pyber_data_df.groupby([“type, “date”]).sum[“fare”]

We then use this data to create a Pivot Table using ``pivot()`` to format our new data for clarity and visual appeal, and using ``loc()`` to limit our data from January 1, 2019 until April 29, 2019. 

	  Reset the Index
	  pyber_data_df = pyber_data_df.reset_index()

	  Create a Pivot Table, Indexing for Date, With Columns for City Type & Fares as Values
	  total_fares_pivot_table =pyber_data_df.pivot( index= ‘date’, columns= ‘type’, values= ‘fare’)

<img src="https://github.com/chrisknox97/pyber_analysis/blob/main/Tables/Pivot%20Table%20Fares%20by%20City%20Date.png" height="400">

The result shows a table displaying each day’s daily fares by city type. But we still need to limit our intended scope to January 2019 through April 2019 and use our daily totals to calculate new weekly totals. 

    Limit the Pivot Table Scope to Specified Dates
    jan_april_2019_fares_df = total_fares_pivot_table.loc[‘2019-01-01’ : ‘2019-04-29’]

    Use Resample() to Reformat the Dates into Weekly Totals
    jan_april_2019_fares_weekly = jan_april_2019_df.resample(‘W’).sum()


<img src="https://github.com/chrisknox97/pyber_analysis/blob/main/Tables/Pivot%20Table%20Fares%20Weekly.png"  height="400">

By doing this, we now have the correct  data from which we can plot the total weekly fares by city type onto a multiple line chart. 

	Import graphing style from Matplotlib
	from matplotlib import style
	style.use(‘fivethirtyeight’)

	Plot the resample DataFrame
	Jan_april_2019_fares_weekly.plot( figsize= (20, 6)

	Add Appropriate Titles
	Plt.title (“Total Fare by City Type”)
	Plt.ylable (“Fare ($USD)” )
	Plt.xlabel (“Month”)
	
![Pyber Graph](https://github.com/chrisknox97/pyber_analysis/blob/main/Analysis/PyBer_fare_summary.png)

By adding these features we see that, regardless of the month, Urban total weekly fares tend to be the highest, and Rural total weekly fares tend to be the lowest. The chart also shows that slight increases in total weekly fares across all city types in late February and slight decreases in Urban and Suburban fares around late March and early April. 

## Summary

With all this data laid out before us, I there are some notable business recommendations to be made to the CEO of Pyber, they are as follows: 

### Increase Pyber Marketing to Rural Regions

The Average Fare Per Ride in Rural Markets was approximately $34 versus $24 in Urban Regions. One could postulate that this is at least partially due to the longer drives needed to reach a final destination. Pyber could capitalize on this market by gradually increasing and incentivizing drivers in rural areas as their current Average Fare per Driver ($55) vastly exceeds those seen in Urban Markets ($17). However, these drivers should be added gradually as there are currently only 78 drivers, meaning any added drivers could significantly impact Average Fare per Driver rapidly without a tact approach. 

### Implement Variable Demand-Based Pricing

As seen in the multiple line chart above, we know that the Total Fares by City Type increase across all city types at the end of February. With this knowledge we charge premium prices during peak operating weeks such as these, as well as during peak operating times of each day.  Uber and Lyft already use variable pricing during busy and high-volume portions of the day, so doing so for weeks as well would not be unheard of for this industry. 

### Decrease Total Drivers in Urban Areas

When we look at our data, we see that Urban drivers have the lowest average fare prices at $16 vs $39 and $55 for Suburban and Rural drivers respectively. We can see this is due in part to the influx of Urban drivers (2,405) outpacing the total number of Urban rides (1,625). The result means that those drivers without a single ride are bringing down the city type’s overall averages. By decreasing the total drivers below over expected ridership, we can stoke demand rather than cloying for it. 
