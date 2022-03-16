![Measurelab logo](/measurelab_black.png)
# Interactive GA4 Schema
This is a description of your project or client, with space below it to record further detail or a list of contents.

### **What & Why**
One of the great features of Google Analytics 4 (GA4) is the ability to pass data into BigQuery (BQ). There are many benefits to this which have already been covered in the previous blog, [10 reasons to export your GA4 data to BigQuery](https://www.measurelab.co.uk/blog/reasons-export-ga4-data-bigquery/). Passing data to BigQuery is no longer just available to enterprise GA360 customers but to anyone using GA4 for free.

What I wanted to create was a simple interactive way to explore how GA4 data is saved in BigQuery. I found the GA360 interactive schema, set up [here](https://storage.googleapis.com/e-nor/visualizations/bigquery/ga360-schema.html), a handy way to know where different elements can be found in the BQ schema, what I'd need to unnest to get to them, and at times found fields I hadn't noticed were available (most recently total.sessionQualityDim).

GA4 has a new schema, different to that of the current schema if you are used to GA360/BigQuery, so this is hopefully useful for new and old GA/BigQuery users.

Finally, as well as outputting an interactive schema I wanted to learn what R was all about and what else may be possible.
How?
[This blog](https://www.cardinalpath.com/blog/using-r-visualize-google-bigquery-export-schemas) had a nice to follow process on how the GA360 interactive map was put together.

In the interest of saving time, I skipped the scraping and tidying of data and simply created a data frame manually using the [GA4 documentation](https://support.google.com/analytics/answer/7029846?hl=en#zippy=%2Cold-export-schema).

This is a summary of the steps involved to create the interactive schema using R, along with links to documentation I found helpful.


### **Step 1**
Install RStudio on my laptop from a local [CRAN Mirror](https://cran.r-project.org/mirrors.html). With R being open source, this is a network of servers to be able to download and install the latest versions of R.


### **Step 2**
Once installed, I then needed to figure out the basics of R. [This video tutorial](https://www.youtube.com/watch?v=BvKETZ6kr9Q) was really helpful in getting a basic understanding of R, from variables to data frames and packages.


### **Step 3**
The interactive schema uses the collapsabletree package. This needed a one off installation on my laptop.  

```R 
install.packages("collapsibleTree")
```


### **Step 4**
Once installed I needed to call the library associated with this package

```R 
library(collapsibleTree)
```


### **Step 5**
Declare the data frame. As above this was pulled together manually to save time. Next steps will be to look at ways this can be automated.

```R 
  ga4_ <- data.frame(
    level = c(
      NA,'event_','event_','event_','event_','event_','event_','event_','event_','event_','event_params','event_params','event_params.value','event_params.value','event_params.value','event_params.value','event_','event_','event_','privacy_info','privacy_info','privacy_info','event_','user_properties','user_properties','user_properties.value','user_properties.value','user_properties.value','user_properties.value','user_properties.value','event_','event_','user_ltv','user_ltv','event_','device','device','device','device','device','device','device','device','device','device','device','device','device','device.web_info','device.web_info','device.web_info','event_','geo','geo','geo','geo','geo','geo','event_','app_info','app_info','app_info','app_info','event_','traffic_source','traffic_source','traffic_source','event_','event_','event_','ecommerce','ecommerce','ecommerce','ecommerce','ecommerce','ecommerce','ecommerce','ecommerce','ecommerce','ecommerce','ecommerce','event_','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items','items'
    ),
    leaf = c(
      'event_','event_date','event_timestamp','event_name','event_params','event_previous_timestamp','event_value_in_usd','event_bundle_sequence_id','event_server_timestamp_offset','event_params','event_params.key','event_params.value','event_params.value.string_value','event_params.value.int_value','event_params.value.double_value','event_params.value.float_value','user_id','user_pseudo_id','privacy_info','privacy_info.ads_storage','privacy_info.analytics_storage','privacy_info.uses_transient_token','user_properties','user_properties.key','user_properties.value','user_properties.value.string_value','user_properties.value.int_value','user_properties.value.double_value','user_properties.value.float_value','user_properties.value.set_timestamp_micros','user_first_touch_timestamp','user_ltv','user_ltv.revenue','user_ltv.currency','device','device.category','device.mobile_brand_name','device.mobile_model_name','device.mobile_marketing_name','device.mobile_os_hardware_model','device.operating_system','device.operating_system_version','device.vendor_id','device.advertising_id','device.language','device.time_zone_offset_seconds','device.is_limited_ad_tracking','device.web_info','device.web_info.browser','device.web_info.browser_version','device.web_info.hostname','geo','geo.continent','geo.sub_continent','geo.country','geo.region','geo.metro','geo.city','app_info','app_info.id','app_info.firebase_app_id','app_info.install_source','app_info.version','traffic_source','traffic_source.name','traffic_source.medium','traffic_source.source','stream_id','platform','ecommerce','ecommerce.total_item_quantity','ecommerce.purchase_revenue_in_usd','ecommerce.purchase_revenue','ecommerce.refund_value_in_usd','ecommerce.refund_value','ecommerce.shipping_value_in_usd','ecommerce.shipping_value','ecommerce.tax_value_in_usd','ecommerce.tax_value','ecommerce.transaction_id','ecommerce.unique_items','items','items.item_id','items.item_name','items.item_brand','items.item_variant','items.item_category','items.item_category2','items.item_category3','items.item_category4','items.item_category5','items.price_in_usd','items.price','items.quantity','items.item_revenue_in_usd','items.item_revenue','items.item_refund_in_usd','items.item_refund','items.coupon','items.affiliation','items.location_id','items.item_list_id','items.item_list_name','items.item_list_index','items.promotion_id','items.promotion_name','items.creative_name','items.creative_slot'
    ),
    variableType = c(
      'RECORD','STRING','INTEGER','STRING','RECORD','INTEGER','FLOAT','INTEGER','INTEGER','RECORD','STRING','RECORD','STRING','INTEGER','FLOAT','FLOAT','STRING','STRING','RECORD','STRING','STRING','STRING','RECORD','STRING','RECORD','STRING','INTEGER','FLOAT','FLOAT','INTEGER','INTEGER','RECORD','FLOAT','STRING','RECORD','STRING','STRING','STRING','STRING','STRING','STRING','STRING','STRING','STRING','STRING','INTEGER','BOOLEAN','RECORD','STRING','STRING','STRING','RECORD','STRING','STRING','STRING','STRING','STRING','STRING','RECORD','STRING','STRING','STRING','STRING','RECORD','STRING','STRING','STRING','STRING','STRING','RECORD','INTEGER','FLOAT','FLOAT','ecommerce.refund_value_in_usd','FLOAT','FLOAT','FLOAT','FLOAT','FLOAT','STRING','INTEGER','RECORD','STRING','STRING','STRING','STRING','STRING','STRING','STRING','STRING','STRING','FLOAT','FLOAT','INTEGER','FLOAT','FLOAT','FLOAT','FLOAT','STRING','STRING','STRING','STRING','STRING','STRING','STRING','STRING','STRING','STRING'
    )
  )
 ```
 
 
### **Step 6**
Next step was to call the package referencing the data frame. This has been developed on from earlier iterations to add a custom fill, height, width and ensure the tree starts in a collabsed state. Other fields can also be declared as part of this with more details [here](https://cran.r-project.org/web/packages/collapsibleTree/collapsibleTree.pdf)

```R  
collapsibleTreeNetwork(ga4_, attribute = "variableType", collapsed = TRUE, zoomable = FALSE, fill = "#66E251", height = 700, width = 1200)
```

### **Step 7**
The final step was to export this to html. This was simply done via the "Export > Save as Web Page" feature in the viewer tab of R studio. 



### **Summary**
At the moment, GA4 is still being developed so I fully expect the current schema to change a few times yet.

In the meantime, I hope this can help anyone who is either transitioning from GA360 to GA4 or to those who are completely new to GA4 and are keen to understand how the data from BigQuery is stored.

