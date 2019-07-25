---
title:  "How to get Google place type icons into your app"
author: "wangela"
---
![img src=??? need imgs plz](/assets/img/img-no-src.png){: .center-image }

Google Maps Platform just [announced new features](https://goo.gle/new-autocomplete-blog) available through its Places Autocomplete service. One of the new features is that for each [prediction returned in the Autocomplete response](https://developers.google.com/places/web-service/autocomplete#place_autocomplete_results), the `types` field will contain more place types (the full list of possible place types is shared in [Place Types Table 1 and Table 2](https://developers.google.com/places/web-service/supported_types) in the documentation).

As a developer, you can make use of this new types information to improve how you display places in Autocomplete. But how do you get the icons for every place type? And what if I want to use those icons in other parts of my UI, like in the results of a nearby search?

![Screenshot of autocomplete with place type icons from the announcement blog post](/assets/img/autocomplete-types.png){: .center-image }

When building the sample above, I ran into a common problem: Where will I get those images? I'm for sure not going to create custom icons myself, and I'd like to use the same icons Google uses to take advantage of the user recognition of those icons from the Google Maps app. Even better if I could have Google host the icons for me so I don't have to maintain them myself...

Luckily, our friends in Material Design have provided [Material Icons](http://google.github.io/material-design-icons/)! They explain the beauty of these icons best:
> Each icon is created using our design guidelines to depict in simple and minimal forms the universal concepts used commonly throughout a UI. Ensuring readability and clarity at both large and small sizes, these icons have been optimized for beautiful display on all common platforms and display resolutions.

Since I built this as a web app, I used the [icon font using Gogole Web Fonts](http://google.github.io/material-design-icons/#icon-font-for-the-web). In the `<head>` of my HTML page, I only needed to add this line:
```html
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```

I wrote a small JavaScript function to check whether a selected type was in the prediction's `types` property, and if it matched one that I wanted to differentiate with a special icon, I populated that icon with the material icon that I thought best matched the type. To find the icon I wanted for each type, I browsed the [Material Icons Library](https://material.io/resources/icons/) and used the name below the icon to reference in my code. By default, I am using the generic place icon.
```javascript
// Checks whether a place prediction's types list contains a specific type
function checkTypes(types, type) {
    return types.includes(type);
}

// Builds a div for each prediction with the appropriate icon
function populate(predictions) {
    predictions.forEach(element => {
        let predictionItem = document.createElement('div');
        let predictionIcon = document.createElement('div');
        let typesList = element.types;
        switch (true) {
            case (typesList.includes("airport")):
                predictionIcon.innerHTML = `<i class="material-icons">local_airport</i>`;
                break;
            case (typesList.includes("restaurant")):
                predictionIcon.innerHTML = `<i class="material-icons">restaurant</i>`;
                break;
            case (typesList.includes("store")):
                predictionIcon.innerHTML = `<i class="material-icons">local_mall</i>`;
                break;
            default:
                predictionIcon.innerHTML = `<i class="material-icons">place</i>`;
        }
        predictionItem.append(predictionIcon);
    });
}
```

This only shows enough cases to cover the types in the example above, but you could go wild with switch cases to match every possible place type.

The Material Icons Guide also provides instructions for self-hosting the font or [image files](http://google.github.io/material-design-icons/#icon-images-for-the-web), getting these icons into your [Android app](http://google.github.io/material-design-icons/#icons-for-android), your [iOS app](http://google.github.io/material-design-icons/#icons-for-ios), and displaying icons in [right-to-left (RTL) language UI](http://google.github.io/material-design-icons/#icons-in-rtl). Best of all, the icons are free for everyone to use under the Apache license version 2.0.