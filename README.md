# O-Ring

## What is a webring?

  A webring (or web ring) is a collection of websites linked together in a circular structure, and usually organized around a specific theme, often educational or social. They were popular in the 1990s and early 2000s, particularly among amateur websites.

  To be a part of the webring, each site has a common navigation bar. It contains links to the previous and next site. By selecting next (or previous) repeatedly, the user will eventually reach the site they started at. This is the origin of the term webring. The select-through route around the ring is usually supplemented by a central site with links to all member sites; this prevents the ring from breaking completely if a member site goes offline. The advantage of a webring is that if the user is interested in the topic on one website, they can quickly connect to another website on the same topic. Webrings usually have a moderator who decides which pages to include in the webring. After approval, webmasters add their pages to the ring by 'linking in' to the ring; this requires adding the necessary HTML or JavaScript widget to their site.

*This explaination was borrowed from [Wikipedia](https://en.wikipedia.org/wiki/Webring)*
## What is O-Ring?
  
  O-ring is a template that anyone can use to create their own webring! Simply follow the steps at the end of this page and you will have your own unique webring! 
  
  O-ring is made of two parts:
  
  1. The JavaScript webring.
  2. The HTML webring.

  Yes, O-Ring is actually made of two independent webrings! You may choose to use **ONLY the JavaScript webring, ONLY the HTML webring, or BOTH webrings together** (both is the recommended method!)
  
  *Some HTML and CSS knowledge is required in order to change the appearence of the JavaScript webring's appearence. Alternatively, if you are not comfortable with HTML or CSS, you can use the the HTML webring*
  
## How does it work?
 
 ### JavaScript webring
 
 Membersites of the webring will put **this code:**
 ```html
 <div id="ringTarget0000"></div><script src="YOURURL/webring.js" type="module"></script>
 ```
 into a webpage's HTML on the website they control.
 
 When a visitor loads a page on a membersite with this script, it will automatically download and run the <a href="#JSRedirect">JavaScript Redirect</a> script hosted from the RingMaster's (you) web server / website.
 
  The script will look at a list of member websites and find the index of the site it is currently running on. The script will then find the previous site in the list, the next site in the list, and a random site in the list. Once the calculations are done, it will insert a snippet of HTML code into the webpage at the location of ringTarget0000 (You will rename this later so it is unique to your webring). The script will also insert a snippet of CSS code to style the HTML and make it look nice.
  
### HTML webring

  Membersites of the webring will put **this code:** 
  ```html
    <div>
      <a href="https://graycot.com/webring/loop-redirect.html?action=prev"> < </a>
      <a href="https://graycot.com/webring/loop-redirect.html?action=list"> ... </a>
      <a href="https://graycot.com/webring/loop-redirect.html?action=home"> Loop Ring </a>
      <a href="https://graycot.com/webring/loop-redirect.html?action=rand"> ? </a>
      <a href="https://graycot.com/webring/loop-redirect.html?action=next"> > </a>
    </div> 
  ```
  into a webpage's HTML on the website they control.
  
  When a visitor on a member site clicks one of these links, they will be directed to a webpage that the RingMaster (you) control. The page you control will run the <a href="#HTMLRedirect">HTML Redirect</a> script.
  
  The script will look at a list of member websites and find the index of the member site that the visitor was on. The script will then find the previous site in the list, the next site in the list, and a random site in the list. Once the calculations are done, it will read the the link after `?action=    ` to determine how and where to redirect the visitor.
  
### 

---
---
---

## Javascript Widget

```html
<div id="ringTarget0000"></div><script src="YOURURL/webring.js" type="module"></script>
```

<h2 id="JSRedirect">JavaScript Redirect</h2>


```js
/* O-Ring v2.0 Copyleft © ALL WRONGS RESERVED © Gray (GGraycot@gmail.com)(https://graystea.neocities.org/)(https://graycot.com/)(discord.gg/invite/JM34yvMaFP).

O-Ring is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License (GPLv3+) as published by
the Free Software Foundation. (http://www.gnu.org/licenses/)*/

// get webring member list
fetch("YOURURL/sites.json")
.then(response => {
   return response.json();
})
.then(data => webring(data));

function webring(data) {

  // get URL of this member site
  let thisSiteURL = window.location.href;
  //init to avoid weird errors
  let thisIndex;
  let i;
  let thisSiteName
  //find this site in member list
  for (i = 0; i < data.webringSites.length; i++) {
    if (thisSiteURL.startsWith(data.webringSites[i].siteURL)) {
      thisIndex = i;
      thisSiteName = data.webringSites[thisIndex].siteName;
      break;
    } 
  }
  //if this site is not in member list, set index to last member of list
  if (thisIndex == undefined) {
  thisIndex = data.webringSites.length-1;
  thisSiteName = 'unknown'
  }
  // find index of site before and after this site. Also compute a random index.
  let previousIndex = (thisIndex-1 < 0) ? data.webringSites.length-1 : thisIndex-1;
  let randomIndex = Math.floor(Math.random() * (data.webringSites.length));
  let nextIndex = (thisIndex+1 >= data.webringSites.length) ? 0 : thisIndex+1;
  // use the indices calculated above to find the corresponding site URL in the member list
  let previousSiteURL = data.webringSites[previousIndex].siteURL;
  let randomSiteURL = data.webringSites[randomIndex].siteURL;
  let nextSiteURL = data.webringSites[nextIndex].siteURL;
  // use the indices calculated above to find the corresponding site name in the member list
  let previousSiteName = data.webringSites[previousIndex].siteName;
  let randomSiteName = data.webringSites[randomIndex].siteName;
  let nextSiteName = data.webringSites[nextIndex].siteName;
  // get webring info from JSON data.
  let webringName = data.webringInfo[0].webringName;
  let webringHome = data.webringInfo[0].webringHome;
  let webringMemberList = data.webringInfo[0].webringMemberList;
  // This is for debugging and building your own HTML widget with the JS variables
  console.log(`previousIndex: ${previousIndex}`);
  console.log(`thisIndex: ${thisIndex}`);
  console.log(`nextIndex: ${nextIndex}`);
  console.log(`randomIndex: ${randomIndex}`);
  console.log(`previousSiteURL: ${previousSiteURL}`);
  console.log(`thisSiteURL: ${thisSiteURL}`);
  console.log(`nextSiteURL: ${nextSiteURL}`);
  console.log(`randomSiteURL: ${randomSiteURL}`);
  console.log(`previousSiteName: ${previousSiteName}`);
  console.log(`thisSiteName: ${thisSiteName}`);
  console.log(`nextSiteName: ${nextSiteName}`);
  console.log(`randomSiteName: ${randomSiteName}`);
  console.log(`webringName: ${webringName}`)
  console.log(`webringHome: ${webringHome}`)
  console.log(`webringMemberList: ${webringMemberList}`)

/*---------------------------------------------------------------- */
           /*               HTML                  */
/*---------------------------------------------------------------- */

/* REPLACE ALL INSTANCES OF "ring0000" WITH THE SAME NAME AND NUMBER 
UNIQUE TO YOUR WEBRING Example: AquaRing3827*/


/* REPLACE "ringTarget000" HERE AND IN THE HTML WITH THE SAME NAME AND NUMBER UNIQUE TO YOUR WEBRING Example: ringTarget3827  */
//insert the HTML widget at the div with the ringTarget#### class.
  let tag = document.getElementById('ringTarget0000');
  tag.insertAdjacentHTML('afterbegin', ` 
  
  <div id="ring0000">
    <div class="left">
      <p> </p>
      <a href = ${previousSiteURL}> ${previousSiteName} </a>
      <a href = ${previousSiteURL}> <<< </a>
    </div>
    <div class="middle">
      <a href = ${webringHome}>${webringName}</a>
      <a href = ${webringMemberList}> ..... </a>
      <a href = ${randomSiteURL}> ??? </a>
    </div>
    <div class="right">
      <p> </p>
      <a href = ${nextSiteURL}> ${nextSiteName} </a>
      <a href = ${nextSiteURL}> >>> </a>
    </div>
  </div>

  `);
// the 2 instances of <p> </p> contain an invisible unicode character. This is only for the example widget formatting.

/*---------------------------------------------------------------- */
           /*               CSS                  */
/*---------------------------------------------------------------- */


  const styles = `
  
  #ring0000{
    display: inline-flex; 
    width: max-content;
    align-items: center;
    padding: 5px 10px;
    border: 1px solid white;
    border-radius: 10px;
    color: white;
    text-align: center;
  }
  #ring0000 a, #ring0000 p {
    display: block;
  }
  #ring0000 .middle{
    padding: 0 1rem;
  }

  `
  // append the CSS to the head of the HTML document
  const styleSheet = document.createElement("style")
  styleSheet.innerText = styles
  document.head.appendChild(styleSheet)

};


/* */
```

---

## HTML Widget

```html
  <div>
    <a href="https://graycot.com/webring/loop-redirect.html?action=prev"> < </a>
    <a href="https://graycot.com/webring/loop-redirect.html?action=list"> ... </a>
    <a href="https://graycot.com/webring/loop-redirect.html?action=home"> Loop Ring </a>
    <a href="https://graycot.com/webring/loop-redirect.html?action=rand"> ? </a>
    <a href="https://graycot.com/webring/loop-redirect.html?action=next"> > </a>
  </div> 
```
  
<h2 id="HTMLRedirect">HTML Redirect</h2>

```js
/* O-Ring v2.0 Copyleft © ALL WRONGS RESERVED © Gray (GGraycot@gmail.com)(https://graystea.neocities.org/)(https://graycot.com/)(discord.gg/invite/JM34yvMaFP).

O-Ring is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License (GPLv3+) as published by
the Free Software Foundation. (http://www.gnu.org/licenses/)*/

// get webring member list
fetch("./sites.json")
.then(response => {
   return response.json();
})
.then(data => webring(data));

function webring(data) {

  // get URL of this member site
  let thisSiteURL = window.location.href;
  //init to avoid weird errors
  let thisIndex;
  let i;
  let thisSiteName
  //find this site in member list
  for (i = 0; i < data.webringSites.length; i++) {
    if (thisSiteURL.startsWith(data.webringSites[i].siteURL)) {
      thisIndex = i;
      thisSiteName = data.webringSites[thisIndex].siteName;
      break;
    } 
  }
  //if this site is not in member list, set index to last member of list
  if (thisIndex == undefined) {
  thisIndex = data.webringSites.length-1;
  thisSiteName = 'unknown'
  }
  // find index of site before and after this site. Also compute a random index.
  let previousIndex = (thisIndex-1 < 0) ? data.webringSites.length-1 : thisIndex-1;
  let randomIndex = Math.floor(Math.random() * (data.webringSites.length));
  let nextIndex = (thisIndex+1 >= data.webringSites.length) ? 0 : thisIndex+1;
  // use the indices calculated above to find the corresponding site URL in the member list
  let previousSiteURL = data.webringSites[previousIndex].siteURL;
  let randomSiteURL = data.webringSites[randomIndex].siteURL;
  let nextSiteURL = data.webringSites[nextIndex].siteURL;
  // get webring info from JSON data.
  let webringName = data.webringInfo[0].webringName;
  let webringHome = data.webringInfo[0].webringHome;
  let webringMemberList = data.webringInfo[0].webringMemberList;

  // Detects whether user clicked the Previous, List, Home, Next, Random, or other link:
  const params = new Proxy(new URLSearchParams(window.location.search), {
  get: (searchParams, prop) => searchParams.get(prop),
  });
  let value = params.action;

  // If the site that the user just came from is not part of the webring, this sets the Previous and Next button to Random.
  if (thisIndex == null) {
    previousIndex = randomIndex;
    nextIndex = randomIndex;
  } 

  // Previous, List, Home, Next, Random, or other actions
  if (value == 'prev') {
      window.location.href = previousSiteURL;
  } else if (value == 'next') {
      window.location.href = nextSiteURL;
  } else if (value == 'list') {
      window.location.href = webringMemberList;
  } else if (value == 'home') {
      window.location.href = webringHome; 
  } else if (value == 'rand') {
      window.location.href = randomSiteURL;
  } else {
      window.location.href = randomSiteURL; //In-case of value == null
  }

};

/* */
```

---

## Instructions / How to implement

  First, choose whether you will include only the JavaScript webring, the HTML webring, or Both.



