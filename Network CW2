# # Project 9: Visualizing city through its businesses: (Abdullahi)

# Cities tend to be mono centric or poly centric (so single downtown or many areas for leisure and shopping). Both the variants have their pros and cons. Some cities have strict bifurcation between where people live, where people work and where people spend their leisure time. London, being an ancient city, shows a mix of polycentric and gentrified arrangement.
# 
# In this project, you will try to visualize the structure of the two cities in terms of business centers, through the foursquare checkin behaviour of its citizens.
# 
# Variant 2: Similarly, using the checkins from Tokyo, try and find if you see spatial clustering of business along types. e.g. do restaurants tend to cluster around each other, or do all theaters fall on the same street.
# Dataset : https://www.kaggle.com/chetanism/foursquare-nyc-and-tokyo-checkin-dataset
# 
# Quantify this phenomenon using some metrics and graphs to differentiate between business types and their clustering. Further, can you predict using the timestamps of checkins, any sort of flow between business types. e.g. you are likely to see a checkin in a bar after a checkin in a restaurant. Make observations, and explore predictive value of such analysis.

# In[1]:


  import pandas as pd
  import numpy as np
  import matplotlib.pyplot as plt
  import os
  import folium
  from folium import plugins
  import webbrowser
  import geopandas as gpd
  from folium.plugins import HeatMap
  Tokyo = pd.read_csv('dataset_TSMC2014_TKY.csv')
  Tokyo


# In[2]:


  category_df = pd.DataFrame(Tokyo.groupby('venueCategory').size().sort_values(ascending=False))
  category_df.to_csv('Result.csv')


# In[3]:


  Tokyo.groupby('venueCategory').size().sort_values(ascending=False)


# In[4]:


  Tokyo.groupby('userId').size().sort_values(ascending=False)


# In[5]:


  newCategory = []
  for i in range(len(Tokyo)):
      newCategory.append('Restaurant' in Tokyo['venueCategory'][i])
  sum(newCategory)


# In[6]:


  Tokyo.groupby(['venueCategory','venueCategoryId']).size()


# In[7]:


  import shapely
  import geopandas as gpd
  geom = [shapely.geometry.Point(xy) for xy in zip(Tokyo.longitude, Tokyo.latitude)]
  gdf = gpd.GeoDataFrame(Tokyo, geometry = geom)
  print(type(gdf))
  gdf

  gdf.to_file('D:\Tokyo_points.shp', encoding="utf-8")plt.figure(figsize=(30,40))
  gdf.geometry.plot()
  plt.show()X=gdf.geometry.x
  Y=gdf.geometry.y
  XX=np.array(X)
  YY=np.array(Y)
  z = list(zip(YY,XX))
  zz=np.array(z)
  print(zz)from sklearn.cluster import KMeans
  y_pred = KMeans(n_clusters=3, random_state=9).fit_predict(zz)
  plt.scatter(zz[:, 0], zz[:, 1], c=y_pred)
  plt.show()schools_map = folium.Map(location=[gdf['latitude'].mean(), gdf['longitude'].mean()], zoom_start=10)
  marker_cluster = plugins.MarkerCluster().add_to(schools_map) 
  tupxy = [tuple(x) for x in gdf[["latitude", "longitude"]].values]
  for coord in tupxy:
      folium.CircleMarker(location=[coord[0], coord[1]],radius=5,color='#3186cc',fill_color='#3186cc',).add_to(schools_map)
  file_path = r"test2.html"
  schools_map.save(file_path)# from folium.plugins import HeatMap
  data = gdf[["latitude", "longitude"]].values.tolist()
  hmap = folium.Map(location=[gdf['latitude'].mean(), gdf['longitude'].mean()],control_scale=True, zoom_start=12)
  hmap.add_child(HeatMap(data, radius=5, gradient={.2: 'blue', .35: 'lime', .6: 'yellow'}))
  file_path = r"test_heatmap.html"
  hmap.save(file_path)gdf_chi = gdf[gdf['venueCategory'] == 'Chinese Restaurant']
  gdf_chiChinese_map = folium.Map(location=[gdf_chi['latitude'].mean(), gdf_chi['longitude'].mean()], zoom_start=12)
  tupxy = [tuple(x) for x in gdf_chi[["latitude", "longitude"]].values]
  for coord in tupxy:
      folium.CircleMarker(location=[coord[0], coord[1]],radius=5,color='#3186cc',fill= True, fill_color='#3186cc',).add_to(Chinese_map)
  file_path = r"test_chi.html"
  Chinese_mapdata = gdf_chi[["latitude", "longitude"]].values.tolist()
  hmap = folium.Map(location=[gdf_chi['latitude'].mean(), gdf_chi['longitude'].mean()],control_scale=True, zoom_start=13)
  hmap.add_child(HeatMap(data, radius=5, gradient={.1: 'blue', .2: 'lime', .3: 'yellow'}))
  file_path = r"test_chi_heatmap.html"
  hmapcluster_map = folium.Map(location=[gdf['latitude'].mean(), gdf['longitude'].mean()], zoom_start=10)

# create a mark cluster object
  marker_cluster = plugins.MarkerCluster().add_to(cluster_map) 

# add data point to the mark cluster
  tupxy = [tuple(x) for x in gdf[["latitude", "longitude","venueCategory"]].values]
  for coord in tupxy:
      folium.Marker(location=[coord[0], coord[1]],icon=None, popup=coord[2]).add_to(marker_cluster)
# add marker_cluster to map
  cluster_map.add_child(marker_cluster)
  file_path = r"test2_cluster_map.html"
  cluster_map.save(file_path)gdf_train = gdf[gdf['venueCategory'] == 'Train Station']
  Train_map = folium.Map(location=[gdf_train['latitude'].mean(), gdf_train['longitude'].mean()], zoom_start=12)
  tupxy = [tuple(x) for x in gdf_train[["latitude", "longitude"]].values]
  for coord in tupxy:
     folium.CircleMarker(location=[coord[0], coord[1]],radius=5,color='#3186cc',fill= True, fill_color='#3186cc',).add_to(Train_map)
  file_path = r"test_train.html"
  Train_map.save(file_path)data = gdf_train[["latitude", "longitude"]].values.tolist()
  hmap = folium.Map(location=[gdf_train['latitude'].mean(), gdf_train['longitude'].mean()],control_scale=True, zoom_start=13)
  hmap.add_child(HeatMap(data, radius=5, gradient={.2: 'blue', .35: 'lime', .5: 'yellow'}))
  file_path = r"test_train_heatmap.html"
  hmap.save(file_path)
# In[8]:


  gdf_ramen = gdf[gdf['venueCategory'] == 'Ramen /  Noodle House']
  Ramen_map = folium.Map(location=[gdf_ramen['latitude'].mean(), gdf_ramen['longitude'].mean()], zoom_start=12)
  tupxy = [tuple(x) for x in gdf_ramen[["latitude", "longitude"]].values]
  for coord in tupxy:
      folium.CircleMarker(location=[coord[0], coord[1]],radius=5,color='#3186cc',fill= True, fill_color='#3186cc',).add_to(Ramen_map)
  file_path = r"test_ramen.html"
  Ramen_map


# In[9]:


  data = gdf_ramen[["latitude", "longitude"]].values.tolist()
  hmap = folium.Map(location=[gdf_ramen['latitude'].mean(), gdf_ramen['longitude'].mean()],control_scale=True, zoom_start=13)
  hmap.add_child(HeatMap(data, radius=5, gradient={.2: 'blue', .35: 'lime', .5: 'yellow'}))
  file_path = r"test_ramen_heatmap.html"
  hmap


# In[10]:


  gdf_bar = gdf[gdf['venueCategory'] == 'Bar']
  Bar_map = folium.Map(location=[gdf_bar['latitude'].mean(), gdf_bar['longitude'].mean()], zoom_start=12)
  tupxy = [tuple(x) for x in gdf_bar[["latitude", "longitude"]].values]
  for coord in tupxy:
      folium.CircleMarker(location=[coord[0], coord[1]],radius=5,color='#3186cc',fill= True, fill_color='#3186cc',).add_to(Bar_map)
  file_path = r"test_ramen.html"
  Bar_map


# In[17]:


  data = gdf_bar[["latitude", "longitude"]].values.tolist()
  hmap = folium.Map(location=[gdf_bar['latitude'].mean(), gdf_bar['longitude'].mean()],control_scale=True, zoom_start=13)
  hmap.add_child(HeatMap(data, radius=5, gradient={.2: 'blue', .35: 'lime', .5: 'yellow'}))
  file_path = r"test_bar_heatmap.html"
  hmap


# In[12]:


  gdf_park = gdf[gdf['venueCategory'] == 'Park']
  Park_map = folium.Map(location=[gdf_park['latitude'].mean(), gdf_park['longitude'].mean()], zoom_start=12)
  tupxy = [tuple(x) for x in gdf_park[["latitude", "longitude"]].values]
  for coord in tupxy:
      folium.CircleMarker(location=[coord[0], coord[1]],radius=5,color='#3186cc',fill= True, fill_color='#3186cc',).add_to(Park_map)
  file_path = r"test_ramen.html"
  Park_map


# In[16]:


  data = gdf_park[["latitude", "longitude"]].values.tolist()
  hmap = folium.Map(location=[gdf_park['latitude'].mean(), gdf_park['longitude'].mean()],control_scale=True, zoom_start=12)
  hmap.add_child(HeatMap(data, radius=5, gradient={.2: 'blue', .35: 'lime', .5: 'yellow'}))
  file_path = r"test_bar_heatmap.html"
  hmap


# In[18]:


  gdf_b = gdf[gdf['venueCategory'] == 'Building']
  B_map = folium.Map(location=[gdf_b['latitude'].mean(), gdf_b['longitude'].mean()], zoom_start=12)
  tupxy = [tuple(x) for x in gdf_b[["latitude", "longitude"]].values]
  for coord in tupxy:
     folium.CircleMarker(location=[coord[0], coord[1]],radius=5,color='#3186cc',fill= True, fill_color='#3186cc',).add_to(B_map)
  file_path = r"test_ramen.html"
  B_map


# In[20]:


  data = gdf_b[["latitude", "longitude"]].values.tolist()
  hmap = folium.Map(location=[gdf_b['latitude'].mean(), gdf_b['longitude'].mean()],control_scale=True, zoom_start=12)
  hmap.add_child(HeatMap(data, radius=5, gradient={.1: 'blue', .2: 'lime', .35: 'yellow'}))
  file_path = r"test_bar_heatmap.html"
  hmap
