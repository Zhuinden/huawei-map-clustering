# Huawei Map Clustering
A high performance marker clustering library for Huawei Map.</br>
Clustering folder is the library.</br>
Have a look at SingleCluster & MultipleCluster folders for easy cluster implementation.

Article for SingleCluster:
https://medium.com/@heydjbaby/100k-clustered-markers-with-huawei-map-ffcba4168727

Article for MultipleCluster:
https://medium.com/@heydjbaby/multiple-clusters-on-huawei-map-%EF%B8%8F-76237fc4469d

|<img src="Screenshots/100k.gif" width="320" height="585">|<img src="Screenshots/multiple.gif" width="320" height="585">|
|:---:|:---:|
| 100K Clustered Markers | 2 x 100k Clustered Markers | Demo App |

## Motivation
Why not use [Huawei built in .clusterable(true)](https://developer.huawei.com/consumer/en/doc/development/HMS-Guides/hms-map-drawonthemap#h2-1586915875534)? Because it's very slow for >100 markers, which causes ANRs. But this library can easily handle thousands of markers (the video above demonstrates the sample application with 100 000 markers running on Huawei P40 Lite).


## Integration
1. Implement `ClusterItem` to represent a marker on the map. The cluster item returns the position of the marker and an optional title or snippet:

```java

class MyItem implements ClusterItem {

    private final LatLng location;

    MyItem(@NonNull LatLng location) {
        this.location = location;
    }

    @Override
    public double getLatitude() {
        return location.latitude;
    }

    @Override
    public double getLongitude() {
        return location.longitude;
    }

    @Nullable
    @Override
    public String getTitle() {
        return null;
    }

    @Nullable
    @Override
    public String getSnippet() {
        return null;
    }
}
```

2. Create an instance of ClusterManager and set it as a camera idle listener using `HuaweiMap.setOnCameraIdleListener(...)`:

```java
ClusterManager<MyItem> clusterManager = new ClusterManager<>(context, huaweiMap);
huaweiMap.setOnCameraIdleListener(clusterManager);
```
3. Populate ClusterManager with items using `ClusterManager.addItems(...)`:

```java
List<MyItem> clusterItems = generateSampleClusterItems();
clusterManager.addItems(clusterItems);
```

4. To add a callback that's invoked when a cluster or a cluster item is clicked, use `ClusterManager.setCallbacks(...)`:

```java
clusterManager.setCallbacks(new ClusterManager.Callbacks<MyItem>() {
            @Override
            public boolean onClusterClick(@NonNull Cluster<MyItem> cluster) {
                Log.d(TAG, "onClusterClick");
                return false;
            }

            @Override
            public boolean onClusterItemClick(@NonNull MyItem clusterItem) {
                Log.d(TAG, "onClusterItemClick");
                return false;
            }
        });
```

5. To customize the icons create an instance of `IconGenerator` and set it using `ClusterManager.setIconGenerator(...)`. You can also use the default implementation `DefaultIconGenerator` and customize the style of icons using `DefaultIconGenerator.setIconStyle(...)`. Refer to CustomIconGenerator where I changed the cluster color via `CustomIconGenerator.createClusterBackground(...)`

## Show your support
This project is completely open source, feel free to make a pull request or report an issue.
<br/>
Give a ⭐️ if this project helped you!

## License

    Copyright 2020-2021 Han Tek Foo

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

