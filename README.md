# CS573 Final Project

## Design
Look at the document.

## Frontend Requirements
1. Three pages.
   
### Main Page
1. Refresh button
2. Search bar - search for topic api
return type:
[
    "v1"
]
3. 20 bubbles

onload api
[
    "v1","v2",..."v416"
]

### Main Page Search
1. After search show topic with one bubble

### Topic Page
1. List of videos - topic api
return type:
[
    {
        title: t1,
        speaker:s1,
        time: ti1,
        views: v1
    },
    {
        title: t2,
        speaker:s2,
        time: ti2,
        views: v2
    }, 
]

2. Images: Title

### Related Videos Network
1. video nodes

video api:
return type:
[
        {
            "title":"v1",
            "url":"url"
        },
        {
            "title":"v2",
            "url":"url"
        },
        {
            "title":"v3",
            "url":"url"
        }
]