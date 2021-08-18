# General information

Includes the most common HTML/CSS/JS snippets to use in Videosync layouts

This applies to all the following snippets:
1. HTML should be added to their respective text fields (e.g. *Page Layout > About section > Content* OR *Page Layout > Hidden stage setting > Text for pre-live presentation*)
2. CSS should be added to *Advanced layout* (usually to the upper field *Register and event page CSS*)
3. JavaScript should be added to *Slides & Settings > Embed own javascript*. Always include <script> `CODE` </script> tags around the JS code.


# Snippets

## Countdown to hidden state

```html
<div class="welcome-text-hidden">
  <h3>Tervetuloa!</h3>
  <div>
    Lähetys alkaa automaattisesti tällä sivulla xx.xx.2021. Tapahtuman alkuun:
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

```css
/* COUNTDOWN */

.welcome-text-hidden {
  font-family: "Inter", sans-serif;
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

```javascript
<script>
/* COUNTDOWN */

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
    var countDownDay = new Date(_eventData.publishingDate).getTime();
    //today
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

        console.log(days);

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

```css
/* Inter as an example */
@import url("https://fonts.googleapis.com/css2?family=Inter&display=swap");

body {
  font-family: "Inter", sans-serif;
}
```

## Topic list to About section

## Changing the order of page elements

## Full width hidden text background image
###Good size background image is 1980x1080
```css
/*Hidden as an example */
#vs-hidden {
      background-size: cover;
}
```
