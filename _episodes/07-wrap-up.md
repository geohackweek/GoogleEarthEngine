---
title: "Wrap-Up"
teaching: 5
exercises: 0
questions:
- "What are the most important GEE resources?"
- "Where do I find answers to my questions about GEE?"
- "What is happening under the hood in Earth Engine?"
objectives:
- "Recap what you've learned"
- Discuss resources for future use and learning
keypoints:
- "Google Earth Engine is a planetary-scale geospatial analysis platform."
- "The Developers Guide has wide collection of documentation and example codes."
- "The Developers Forum can be used to post questions or answer questions about GEE code."
- "The Code Editor and GEE website contain many resources including the data catalog, code documentation and places for user feedback."

---

## Closing Thoughts

In this tutorial, you learned
- [1. Introduction to Google Earth Engine](https://geohackweek.github.io/GoogleEarthEngine/01-introduction/): A basic overview of Google Earth Engine (GEE)
- [2. Code Editor](https://geohackweek.github.io/GoogleEarthEngine/02-code-editor/): How to navigate the code editor
- [3. Accessing Satellite Imagery](https://geohackweek.github.io/GoogleEarthEngine/03-load-imagery/): How to work with regional scale satellite imagery
- [4. Temporal and Spatial Reducers](https://geohackweek.github.io/GoogleEarthEngine/04-reducers/): How to aggregate images through both time and space
- [5. Classifying Satellite Imagery](https://geohackweek.github.io/GoogleEarthEngine/05-classify-imagery/): How to classify imagery for different land uses
- [6. Time Series](https://geohackweek.github.io/GoogleEarthEngine/06-time-series/): How to interactively plot a time series

GEE was designed with a disciplinary scientist in mind (that's you!). Don't hesitate to dig into the documentation, start discussions on the forum, suggest datasets and give feedback along the way.
<br>
<br>

## Bookmark These Webpages
The JavaScript API and online code editor are the best way to develop processing algorithms and get help from the forums. The Python API is generally recommended for production level implementation of developed algorithms.

Because of this, working with GEE is easier with quick access to the online elements. We recommend adding these bookmarks to your web browser for easy use:

- [Code Editor](https://code.earthengine.google.com/): Where you go to write code.
- [Data Catalogue](https://code.earthengine.google.com/datasets/): Details on the available datasets
- [Developers Guide](https://developers.google.com/earth-engine/): the closest thing to a manual
- [Developers Forum](https://groups.google.com/forum/?fromgroups#!forum/google-earth-engine-developers): Where you go to post questions
  - There is also a link to the forum from the "Help" drop down menu in the code editor, but a bookmark can be nice for frequent use
  - The Google Team is extremely active in the forums and are a vital resource. GEE's documentation isn't always complete, so if you're stuck on something, ask!
  - **Recommendation:** Set your email delivery preference to "Send daily summaries." No one wants to receive an email for each post to the forum, but scanning the daily digest for topics you anticipate needing in the future is a great way to pre-emptively find code examples for your work. It's also likely you'll discover a use you didn't know existing through other folks posts.
  - **Tip:** When posting questions to the forum, you'll get the best help if you include a link to your current script that others are able to run in order to help you troubleshoot. Dummy examples can work great, and if you share your actual workspace, be sure that any assets or fusion tables loaded in your script are publicly shared

## Additional Learning Resources
There are several educational and training resources available to continue learning about GEE.

- [GEE tutorials](https://developers.google.com/earth-engine/tutorials)
- [GEE EDU page](https://developers.google.com/earth-engine/edu)
  - Educational resources: provides remote sensing curricula developed with GEE
  - Training resources: Provides resources for those teaching others to use GEE. This includes a general introductory Powerpoint and the Beginner and Intermediate Workshop materials run by GEE

## Going Further
- [Debugging Guide](https://developers.google.com/earth-engine/debugging)
  - Posted by popular demand. An EXCELLENT resource. **Debug a programmer's code, and you appease her forum posts for the script. Teach a programmer to debug, and you appease her forum posts for a lifetime.**
 - [The "Under the Hood" forum thread](https://groups.google.com/forum/#!searchin/google-earth-engine-developers/benefits$20of$20python%7Csort:relevance/google-earth-engine-developers/LWHTFSH9FRk/NGxiEQ5KEQAJ)
   - If you continue to use GEE, you will inevitably begin to have questions about how GEE works under the hood in (possibly vain) attempts to help optimize your workflow. Sometimes operations time out or don't run. In general, GEE's inner workings are quite opaque. Typical fixes and approaches that data scientists use don't really apply in GEE's framework. This thread provides some useful information about what users can and cannot control. Highly recommended reading for regular GEE users.
- [Gorelick et al. 2017](http://www.sciencedirect.com/science/article/pii/S0034425717302900)
  - The Earth Engine Team recently published an overview of the GEE platform. It also provides nice tables of currently available datasets and available functions.
