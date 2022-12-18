# Generating a Network from a Wikipedia Page!

<center><img width="800" src="images/header_image.png"></center>

This project has the goal to generate a network from a single `Wikipedia` page (seed page) and to analyze how the pages are connected to each other based on different metrics. 

Due to the amount of computational power and time to process all data, we are only exploring the network until the second layer, which already include a lot of nodes and connections to be analyzed. 

Another goal of this project is to create a pipeline that receives a starting page from wikipedia and includes all the processing phases. 

It's important to mention that this project is a collaboration between the students [Aryel Medeiros](https://github.com/aryelmedeiros) and [Deborah Moreira](https://github.com/deborahmoreira) 

>  :art: **Interact with graph using [Retina/Gist](https://ouestware.gitlab.io/retina/beta/#/graph/?url=https%3A%2F%2Fgist.githubusercontent.com%2Fdeborahmoreira%2F932375630c4c0d0642d44405bc8409c4%2Fraw%2Fabceff5bb96e483ac59c0e832bde935b755b750b%2Fvertebrate_gephi.graphml&r=d&sa[]=co&sa[]=r&ca[]=g%20e-s&ca[]=g%20s-s&ca[]=g-s&ca[]=e-s&ca[]=cl-s&ca[]=h-s&ca[]=b-s&ca[]=m-s&ca[]=i-s)**

## Part I - Build the network and explore it's metrics

* :movie_camera: Quick explanation about this section [![Open in Loom](https://img.shields.io/badge/-Video-83DA77?style=flat-square&logo=loom)](https://www.loom.com/share/a8a10b1272a34b15a4d5677c458f6a92)

### Pipeline

<p align='center'>
<img src='./images/Pipeline.png'>
</p>

The suggested data pipeline is consisted of 4 explicit steps: `Data Collecting`, `Data Cleaning`, `Truncate` and `Explore the Network`. This last one is actually a "condensed" step that includes other tasks that analyze the network and generate graphs according to determined metrics. 

To define the pipeline we create a class and define the functions that represent each task as well as the corresponding task that it depends on to operate. This assures that the tasks will be executed in the correct order. 

#### Data Collecting 
	
The first task of the pipe line receives the first wikipedia page to be explored. Then, using the wikipedia library the web scraping is performed to extract all the referenced pages in previous page to define the new nodes and connections of the network. 

Also, because of the numerous amount of pages that are connect and the huge amount of time that would take to process other layers of the network, we are only extracting nodes until the second layer. Moreover, we stipulated pages that are end point to the extraction by manually exploring the network, these pages are defined as `STOP` and should represents leaf nodes or nodes that possibly are not relevant to the original topic.

<p align='center'>
<img src='./images/Wikipedia Page.png'>
</p>

The seed page that we decided to explore in this project is the Wikipedia Page about Vertebrate. After the initial extraction of the network until the second layer, there were found around 2.8k nodes and 68k edges. 

#### Data Cleaning 

After the initial definition of the nodes on the netwok, now we need to eliminate all duplicate nodes, nodes that are actually the same page and topic, the only difference would be the title having an extra 's' or a '-'. Not only duplicate nodes, but we also want to remove all the loop edges, meaning a page that refers to itself. 

#### Truncate 

On this step of the pipeline, a function is responsible to filter all nodes that have a degree less or equal to 2, since these nodes don't represent such important nodes. In the first step of the data collecting, there was `2.8k nodes and 68k edges`. After removing all the duplicates, loop edges and truncating the network,  we ended up with around `7005 nodes and  42038 edges.`

#### Show Edges
The total number of nodes and edges after cleaning the data is actually a dedicated simple task of the pipeline.

#### Explore the Network

As previously mentioned, this step of the pipeline is in fact a group of subtasks responsible to analyze the network by some metrics involving: degree, closeness, betweenness, eigenvector, centrality distributions, k-core and k-shell.


##### :bar_chart: Graph Metrics 

The first analysis of the network performed consist in evaluating the nodes folling such metrics: 

 - **Degree centrality**: the number of neighbors (connections) of a node.
 - **Closeness centrality**: distance of a node to all other nodes of the network. A lower value of this metric is iqual to a longer distance.
 - **Betweenness centrality**: the position of a node on the shortest path.
 - **Eigenvector centrality**: measure the importance of a node based on its neighbors. 

After passing our network to this task, we obtained the following image to represent all 4 metrics explained above: 

<p align='center'>
<img src='./images/all_centralities_measures.jpeg'>
</p>

##### :bar_chart: Centrality Distributions

The analysis performed on this task is based on two density  functions: `Probability Density Function`(PDF) and `Cumulative Density Function` (CDF). Functions that are applied over the metric of degree, previously explained.

 - **Probability Density Function** (PDF): gives the probability of a node having the degree **equal** to X.
 - **Cumulative Density Function** (CDF): gives the probability of a node having the degree **equal or less** to X.



<p align='center'>
<img src='./images/pdf.png'>
</p>

According tho de PDF curve for degree metric above, it's possible to see that almost 100% of nodes on the network have a degree less than 100. The CDF curve also confirms this evaluation: 

<p align='center'>
<img src='./images/cdf.jpeg'>
</p>

##### :bar_chart: All Centrality Distributions

This task compare all the metrics.

<p align='center'>
<img src='./images/comparing_centralities.png'>
</p>

Taking a look at the image displayed above, it's possible to see areas that represent clusters in the network. Furthermore, we can say that Eigenvetor distribution has almost a linear look. 


##### :bar_chart: K-Core and K-shell 

For the core decomposition of the network the k-core and k-shell metrics are used on this task. The first one indicates subnet  with all nodes having at least k neighbors and the second one represents the shell, nodes that have to be eliminated to reach a k-core. 

First we discovered how many k-cores the network had and then we displayed in blue and the k-shel is displayed in red. 

<p align='center'>
<img src='./images/core_and_shell.png'>
</p>

## Part II - Different ways to draw the network

For the second part of this project we used the `.grapml` output file of our pipeline and some other powerful tools such as [Gephi](https://gephi.org/), [Retina](https://ouestware.gitlab.io/retina/beta) and [Gephisto](https://jacomyma.github.io/gephisto/) to visualize and display our network in different layouts, colors and shapes, which help us to better understand what was builded in previous section.

### Gephi

With this powerful tool it's possible to import the pipeline's `.grapml` output file  and calculate many other metrics easily. Futhermore, this open-source tool allowed us to display our network with specific layouts, represent the color of nodes based on a metrics and use mutiple filters as needed. With the correct `plug-in` installed, `Gephi` is ablle to export the enhanced network to a `html` / `.js` template, this output file is udes to deploy it into a page here on this repository.

* :globe_with_meridians: See the network as a HTML page [![Open in git](https://img.shields.io/badge/webpage-open%20here-green)](https://deborahmoreira.github.io/data_structure_ii/wikipedia_network/network/)

<p align='center'>
<img src='./images/filtered1.png'>
</p>

To generate the image above we first applied the ForceAtlas 2 layout (a laytout based on phisycs laws) to the network, with the atribute `Dispersion = 10` to better separete nodes from each other. After that, the "magic" started to happen when we paited the nodes using **in-degree** metric and changed their size based on each node's **overall degree**. The **in-degree** metric is the number of edges that reach a node, meaning the times of a page is referenced by other pages. Expansions were also performed on the network to enhance the overall look.


The nodes were first paited with the following colors:
* nodes with degree `2` : black  
* nodes with degree `3`: light blue 
* nodes with degree `4`: green  
* nodes with degree `5`: turquise  
* nodes with degree `6`: brown
* nodes with degree `7`: orange
* nodes with degree `8`: beige
* nodes with degree `9`: light pink 
* nodes with degree `10`: pink 
* other nodes: gray

To better group nodes with higher degrees filters were applied, selecting nodes within a range and painting them with a color as it follows:
* nodes with degree `10 to 38 `: deep purple
* nodes with degree `38 to 86` : yellow
* nodes with degree `higher than 86` : deep red
	
Also, the nodes with the highest degrees (deep red) were labed. As you can see, these steps are intrisic to each network.

In addition, we created a different workspace inside `gephi` to generate another view of the network (to use as header of this document), now running the **community metrics** to paint the nodes. Then we combined the **modularity** filter with the **betweenness centrality** filter to decide which nodes would be labeled. Consequently, the folloing representation was generated:  

<center><img width="800" src="images/header_image.png"></center>

### Retina

Retina is another interesting tool, actually a plugin of Gephi, where we can publish our network on famous world wide web and let anyone interact with it. The link to the page created it's on begin of this readme file and also bellow:

>  :art: **Interact with graph using [Retina/Gist](https://ouestware.gitlab.io/retina/beta/#/graph/?url=https%3A%2F%2Fgist.githubusercontent.com%2Fdeborahmoreira%2F932375630c4c0d0642d44405bc8409c4%2Fraw%2Fabceff5bb96e483ac59c0e832bde935b755b750b%2Fvertebrate_gephi.graphml&r=d&sa[]=co&sa[]=r&ca[]=g%20e-s&ca[]=g%20s-s&ca[]=g-s&ca[]=e-s&ca[]=cl-s&ca[]=h-s&ca[]=b-s&ca[]=m-s&ca[]=i-s)**

With Retina, we explored the same network only now using different metrics to colour the nodes and therefore further analyze releshionships between nodes. The following metrics were explored and explained in a video linked bellow: 
* Out Degree 
* Eccentricity 
* Closeness Centrality 
* Betweeness Centrality

* :movie_camera: Exploratory analysis using few metrics on Retina [![Open in Loom](https://img.shields.io/badge/-Video-83DA77?style=flat-square&logo=loom)]()

### Gephisto

Gephisto it's another way to draw our network using a web based tool. When our `.grapml` is uploaded the result it's the image bellow.

<p align='center'>
<img src='./images/gephisto.jpeg'>
</p>

In this representation, automatically generated in Gephisto, the size of a node depend on out-degree. The following collors represent a descending order of in-degree: 
* nodes with degree = 6 : yellow
* nodes with degree = 5 : red
* nodes with degree = 4 : green
* nodes with degree = 3 : pink
* nodes with degree = 2 : blue

The predominant grey color means all other degrees non represented with the mentioned collors. 

Our first shell is composed by that nodes with degree = 2, it's a filter on our pipeline and we can call them leaves (in blue). Examining few displayed labels, we can see that they are nodes with degree = 2, that is, this visualization emphasizes first shell. 

In addition, Gephisto always choose different ways to draw the graph, so we can not exacly establish a correlation between those names. However, we can notice that most names refers to scientific names to identify organisms (species followed by genus).

## References

([Ivanovitch's Repository](https://github.com/ivanovitchm/datastructure))
