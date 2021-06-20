 <h1 align="center">
 <br>
    Movie Vue <img width = "32px" src = "https://raw.githubusercontent.com/Aveek-Saha/MovieVue/master/public/assets/icon/favicon.png">
</h1>
<h3 align="center"> Check out new new and popular movies on your phone. Built with Ionic and Vue. <h3>


# Usage

![Demo](https://raw.githubusercontent.com/Aveek-Saha/MovieVue/master/screen.gif)

Download the APK and install it on your Android device

# Features
1. A slide out menu from where you can see the different movie lists, Now Playing, Popular, Upcoming, or Top Rated.
1. Each movie will have a background image, the title, the average rating of the movie and the description.
1. Infinite scroll, once you reach the end of the page, new content automatically loads.
1. Swipe down on any page to refresh it.
1. Show region specific movies.

## [Download](https://github.com/Aveek-Saha/MovieVue/releases)

# Develop
Open a live development preview on [`localhost:8100`](http://localhost:8100/)

```
npm install -g @ionic/cli
cd MovieVue
npm i
ionic serve
```

# Build

Create an Android build

```
ionic build
ionic cap add android
```

Open the project in Android Studio
```
ionic cap open android
```