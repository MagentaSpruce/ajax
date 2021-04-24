# ajax
30 Day JS Projects Challenge
This project shows how to filter data from a JSON doc for user search requests.

This project helped me to learn and understand better:

1) JSON data format
2) RegExp method
3) fetch() request
4) Array methods
5) Creating functions
6) 
There are no known bus assiciated with this project. No improvements are slated. It is mostly conceptual in purpose, although, it could be retooled and repurposed with relative ease in various use cases.

A simple walkthrough and explanation of the code is below:

First fetch the data then convert the data from JSON to JS using an empty array and the spread operator. 
```JavaScript
  const endpoint = 'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json';

  const cities = [];

  fetch(endpoint).then(blob => blob.json()).then(data => cities.push(...data));
```

Create a function to filter down the array upon search request. g = global & i = insensitive
```JavaScript
 function findMatches(wordToMatch, cities){
    return cities.filter(place => {
      //if city or state, then regex matches what was searched
      const regex = new RegExp(wordToMatch, 'gi');
      return place.city.match(regex) || place.state.match(regex)
    })
  }

```

Create the display function to get called on value changes. Select the inputs and add listeners.
```JavaScript
  function displayMatches(){
    const matchArray = findMatches(this.value, cities);
    const html = matchArray.map(place => {
      return `<li>
        <span class="name">${place.city}, ${place.state}</span>
        <span class="population">${place.population}</span>
        </li>`
    }).join('');
    suggestions.innerHTML = html;
  }

  const searchInput = document.querySelector('.search');
  const suggestions = document.querySelector('.suggestions');

  searchInput.addEventListener('change', displayMatches);
  searchInput.addEventListener('keyup', displayMatches);
```

Format the numbers and highlight the keywords. Inside the map function before the return another regex can be created to match the city name and use the regex to replace the word that it matches with a span.
```Javascript
function displayMatches() {
  const matchArray = findMatches(this.value, cities);
  const html = matchArray.map(place => {
    const regex = new RegExp(this.value, 'gi');
    const cityName = place.city.replace(regex, `<span class="hl">${this.value}</span>`);
    const stateName = place.state.replace(regex, `<span class="hl">${this.value}</span>`);
    return `
      <li>
        <span class="name">${cityName}, ${stateName}</span>
        <span class="population">${numberWithCommas(place.population)}</span>
      </li>
    `;
  }).join('');
  suggestions.innerHTML = html;
}

```

Create a function to insert commas.
```JavaScript
function numberWithCommas(x) {
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}
```

***End walkthrough
