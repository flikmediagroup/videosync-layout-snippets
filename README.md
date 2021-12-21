# General information

Includes **FAQ** and the most common **HTML/CSS/JS snippets** to use in Videosync layouts

##

# FAQ

First, responses to some frequently asked questions about Videosync layouts:

##

### What is CSS class or id?

A class is used in HTML like this: `<div class="container">content</div>`.

In CSS, this class can be referred like this to change its font color, for example: `.container { color: red; }`

ID is basically the same, but with different syntax: `<div id="container-id">content</div>`.

In CSS: `#container { color: red; }`

So in CSS, class uses dot (.) and id uses hashtag (#).

##

### What is a good resolution for background images?

1920x1080 is usually fine. If the image size is several Mb's, you should resize it using Videosync image cropper or external tools.

##

### How do I hide/show chat messages (or any other element)?

### Chat:

There is now a toggle within messageboard settings **"Hide published messages on viewing page"**.

If it is an old event:

**To show**

1. Go to **Page Layout > Advanced layout**
2. (Ctrl+f) search for `#chatboard-comments { display: none; }`
3. Remove `display: none;`

**To hide**

1. Go to **Page Layout > Advanced layout**
2. Add the following code `#chatboard-comments { display: none; }`

### Any element

The `display: none;` rule can be applied to all elements on the page. You just have to find out the name of the required element using the browser's inspector or guess from the list below.

```css
/* Names of the most common main elements in event page */

#vs-header {
  /* Top bar with the event title */
}

#vs-about-below-video {
  /* About section directly below video player */
}

#vs-about-section {
  /* Normal about section */
}

#vs-registration {
  /* Registration form area */
}

#vs-presentation {
  /* General video player & slide (& chat next to video) area*/
}

#vs-reactions {
  /* Emoji reactions below video player */
}

#vs-thumbnails {
  /* Slide thumbnails carousel below video & slide */
}

#vs-attachments {
  /* Attachments list */
}

#vs-hidden {
  /* Hidden state placeholder for the player/slide area. Usually contains text "Live is paused" etc.
  Applies to pre-live, paused and hidden-od states. */
}

#vs-messageboard {
  /* Chat / Messageboard either next to video player or below it as an own section */
}

#vs-links {
  /* Link grid */
}

#vs-chapters {
  /* Chapters list */
}

#vs-chapter-select {
  /* Chapter select dropdown in on-demands */
}

#vs-footer {
  /* Mainfooter */
}

#vs-videosync-footer {
  /* Videosync's own small footer on the bottom  with terms, privacy policy, copyright notice */
}

#vs-users-list {
  /* Participants list as a section on the page */
}

/* Notify the dev team if something should be added to this list */
```

##

### I can't find an element, for example "Speakers list" anywhere on the admin page. Where is it and how can I edit it?

It is most likely custom made HTML structure in the about section. Check it out and look for HTML tags containing the information you're looking for:

`<tags class="custom">INFO: Speaker Name</tags>`

Now, it's easy to change the necessary information to what you want, e.g. _Speaker Name -> Another Alias_ without touching the HTML code.

The styles related to this element can also be found by searching the _.custom_ class name within the **Advanced layout**:

```css
.custom {
  color: red;
}
```

##

# Snippets

Guide to adding the following snippets to Videosync:

1. HTML should be added to their respective text fields (e.g. **Page Layout > About section > Content** OR **Page Layout > Hidden stage setting > Text for pre-live presentation**)
2. CSS should be added to **Page layout > Advanced layout** (usually to the upper field **Register and event page CSS**)
3. JavaScript should be added to **Slides & Settings > Embed own javascript**. Always include <script> `CODE` </script> tags around the JS code.

## Countdown to hidden state

HTML:

```html
<div class="welcome-text-hidden">
  <h3>Tervetuloa!</h3>
  <div>
    Lähetys alkaa automaattisesti tällä sivulla <span id="date-span"></span>.
    Tapahtuman alkuun:
  </div>
</div>
<div class="hidden-text-container">
  <div class="countdown-container">
    <div><span id="days"></span><span class="count-span">Päivää</span></div>
    <div><span id="hours"></span><span class="count-span">Tuntia</span></div>
    <div>
      <span id="minutes"></span><span class="count-span">Minuuttia</span>
    </div>
    <div>
      <span id="seconds"></span><span class="count-span">Sekunttia</span>
    </div>
  </div>
</div>
```

CSS:

```css
/* COUNTDOWN */

.welcome-text-hidden {
  display: flex;
  flex-direction: column;
  justify-content: center;
  opacity: 0.9;
}

.welcome-text-hidden h3 {
  margin: 0;
  font-size: 24px;
}

.welcome-text-hidden div {
  width: 300px;
  font-size: 16px;
  margin: 33px 12px 12px 0;
  line-height: 140%;
}

.countdown-container {
  flex-wrap: wrap;
  display: flex;
}

.countdown-container div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: #0a121f;
  min-width: 100px;
  min-height: 108px;
  margin: 12px 12px 12px 0;
  opacity: 0.9;
  border-radius: 10px;
}

.count-span {
  line-height: 1;
  margin-top: 14px;
  font-size: 14px;
}

#days,
#hours,
#minutes,
#seconds {
  font-weight: 700;
  font-size: 40px;
}
```

JavaScript:

```javascript
<script>
/* COUNTDOWN */

var url = window.location.pathname;

function addCountDown(e) {
  var videoEl = document.getElementById("bitmovin-player");
  var regEl = document.querySelector("#vs-registration");
  var regElContainer = document.querySelector(
    "#vs-registration .content.content-center"
  );
  var hiddenContainer = document.querySelector("#days");
  var registerForm = document.querySelector(
    "#vs-registration .content.content-center .register-form"
  );
  if (!hiddenContainer && !videoEl && !regElContainer) {
    window.requestAnimationFrame(addCountDown);
  } else if (!videoEl) {
    var date = new Date(_eventData.publishingDate);
    var countDownDay = date.getTime();
    var dateSpan = document.querySelector("#date-span");
    dateSpan.innerHTML = date.getDate() + "." + (date.getMonth() + 1) + "." + date.getFullYear();
    // today
    var now = new Date().getTime();
    if (countDownDay === now || countDownDay < now) {
      var hiddenTextContainer = document.querySelector(
        ".hidden-text-container"
      );
      hiddenTextContainer.style.display = "none";
    } else {
      setInterval(function () {
        var daysEl = document.getElementById("days");
        var hoursEl = document.getElementById("hours");
        var minutesEl = document.getElementById("minutes");
        var secondsEl = document.getElementById("seconds");
        // distance
        var currentNow = new Date().getTime();
        var distance = countDownDay - currentNow;
        // setting distance time to days, hours, minutes, seconds
        var days = Math.floor(distance / (1000 * 3600 * 24));


        var hours = Math.floor((distance % (1000 * 3600 * 24)) / (1000 * 3600));
        var minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
        var seconds = Math.floor((distance % (1000 * 60)) / 1000);
        // display result
        if (days < 10) {
          daysEl.innerHTML = "0" + days + " ";
        } else {
          daysEl.innerHTML = days + " ";
        }
        if (hours < 10) {
          hoursEl.innerHTML = "0" + hours + " ";
        } else {
          hoursEl.innerHTML = hours + " ";
        }
        if (minutes < 10) {
          minutesEl.innerHTML = "0" + minutes + " ";
        } else {
          minutesEl.innerHTML = minutes + " ";
        }
        if (seconds < 10) {
          secondsEl.innerHTML = "0" + seconds + " ";
        } else {
          secondsEl.innerHTML = seconds + " ";
        }
        if (seconds < 0) {
          var hiddenTextContainer = document.querySelector(
            ".hidden-text-container"
          );
          hiddenTextContainer.style.display = "none";
        }
      }, 1000);
    }
  }
}

if (!url.includes("register")) {
  addCountDown();
}
</script>
```

## Using a Google font

CSS:

```css
/* Inter as an example */
@import url("https://fonts.googleapis.com/css2?family=Inter&display=swap");

body {
  font-family: "Inter", sans-serif;
}
```

## Topic list to About section

HTML:

```html
<ul>
  <li>Topic 1</li>
  <li>Topic 2</li>
  <li>Topic 3</li>
  <li>Topic 4</li>
  <li>Topic 5</li>
</ul>
```

Looks like this:

<ul>
  <li>Topic 1</li>
  <li>Topic 2</li>
  <li>Topic 3</li>
  <li>Topic 4</li>
  <li>Topic 5</li>
</ul>

## Changing the order of page elements

```css
/* Add this to make the "order" rules work */
.vs-mainwrapper {
  display: flex;
  flex-direction: column;
}

/* Add the rule "order: " to all necessary elements followed by the
respective number in which order you want them to be */
#vs-header {
  order: 0;
}

#vs-about-section {
  order: 2;
}

#vs-presentation {
  order: 1;
}
```

## Full width background image for the hidden text

_Good size for a background image is 1980x1080_

```css
/* Hidden as an example */
#vs-hidden {
  background-size: cover;
}
```

## Produced by FLIK -footer

Promotional footer to be used in events produced by FLIK!

Copy this code to the custom JavaScript section:

```html
<script src="https://cdn.videosync.fi/events/_common/flik-footer.js"></script>
<link rel="stylesheet" href="https://cdn.videosync.fi/events/_common/flik-footer.css">
```
