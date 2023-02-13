---
layout: post
title: "I asked ChatGPT to help speed up my React code"
date: 2023-02-13 12:30
tags: javascript chatgpt ai
navigation: true
class: post-template
current: post
---

I finally got around to trying ChatGPT. I'm sure there are many ways of using this technology, but I think it can be a great assistant to help <em>you</em> improve your own code. Today I'll share how I used it in my workflow.

The pokedex app I made a few months ago was loading really slowly, and I was struggling to speed it up. Since this is just a small demo app I figured it would be a good use case to test out ChatGPT.

The problem seemed to be localized to one file, where I was fetching the data for each pokemon and mapping it onto the screen. I asked ChatGPT "This React code is loading slowly. How can I speed it up?", and pasted all the code from my file.

I won't share it all here, but this function is what originally fetched the data:

```js
const fetchPokemonData = async (pokemonNames) => {
  for (const pokemon of pokemonNames) {
    //do a fetch
    const url = `https://pokeapi.co/api/v2/pokemon/${pokemon}/`;

    await axios.get(url).then((res) => {
      //save this pokemon's data to sessionStorage

      sessionStorage.setItem(
        `${pokemon}`,
        JSON.stringify({
          abilities: res.data.abilities,
          types: res.data.types,
          height: res.data.height,
          weight: res.data.weight,
          id: res.data.id,
          sprites: res.data.sprites,
        })
      );
    });
  }
  await getCachedData(allPokemon);

  return true;
};
```

Looking back now, I can see that using a for loop and making an API call in each iteration isn't the most efficient way of doing this.

ChatGPT responded with a list of potential fixes:

> There are several ways to improve the performance of this code:
>
> 1.Use local storage instead of session storage: Local storage has a larger size limit compared to session storage and can persist data even after the browser is closed. This will eliminate the need for making repeated API calls for the same data.
>
> 2.Reduce the number of API calls: You can reduce the number of API calls by fetching data for multiple Pokemon in a single call instead of making separate API calls for each Pokemon.
>
> 3.Use memoization: You can memoize the result of expensive calculations so that they are only re-computed when their inputs change. This can significantly improve performance in cases where the same calculation is performed multiple times with the same inputs.
>
> 4.Use conditional rendering: You can use conditional rendering to avoid unnecessary rendering of components. For example, you can avoid rendering the Loading component once the data has been loaded.
>
> 5.Use code splitting: You can split the code into smaller chunks so that only the necessary code is loaded and the rest can be loaded as needed, improving the initial loading time of the page.

Some of these didn't quite work for me. I specifically wanted session storage, not local storage, because I didn't want users to have to clear their browsing data to get rid of the pokemon data. Session storage clears when the window is closed. Memoization could have worked, but I was already using session storage and it seemed unnecessary to re-write the storage related code. Besides, it seemed like the problem had more to do with the fetch itself.

I asked ChatGPT how I could reduce the API calls, and it told me to use `Promise.all()`. I don't know how I didn't think of that before.

It even gave me some new code, and explained how it worked.

> You can fetch multiple Pokemon at once by making multiple API calls in parallel using Promise.all(). Here's how you can modify the fetchPokemonData function to do this:

```js
const fetchPokemonData = async (pokemonNames) => {
  const requests = pokemonNames.map((pokemon) => {
    const url = `https://pokeapi.co/api/v2/pokemon/${pokemon}/`;
    return axios.get(url);
  });

  try {
    const responses = await Promise.all(requests);
    responses.forEach((res) => {
      sessionStorage.setItem(
        `${res.data.name}`,
        JSON.stringify({
          abilities: res.data.abilities,
          types: res.data.types,
          height: res.data.height,
          weight: res.data.weight,
          id: res.data.id,
          sprites: res.data.sprites,
        })
      );
    });
  } catch (error) {
    console.error(error);
  }
  getCachedData(allPokemon);
  return true;
};
```

> This will send multiple requests in parallel, and once all of them have resolved, the data will be saved in sessionStorage and getCachedData will be called to retrieve the data.

All I had to do was update this one function. I tried it out, and the load time was so much faster.

### Conclusions

The prospect of AI taking our jobs is a bit scary, and as ethical technologists it is important that we don't create harmful AI.

That being said, I do think there is promise for AI acting as a helper, rather than something that takes over our work. I recently heard someone compare AI to the original desktop computers. People feared that computers would make many jobs obsolete, and I suppose for some it has. But we generally use computers to assist us with tasks, rather than do it all for us. I think AI can be used in the same way. Work has become less analog and more digital, for better or worse. Work has changed and evolved over time with the help of technology.

ChatGPT didn't make the app for me, I did. It helped me like a coworker might help me - I came to it with a problem, and it gave me some pointers to improve. I've absolutely had coworkers write some code for me to use while helping me with a problem. ChatGPT also didn't have just one answer to help me. It gave me suggestions, which I needed to analyze myself to determine which one could work best given my requirements.

Which brings me to another important point - you (or your managers) need to know what the requirements are to make anything. ChatGPT can't make an entire software system by itself (yet...?).

I think it can also be helpful for my learning. Now I can look into how `Promise.all()` works and use it more in the future. I was reminded of some faster alternatives to for loops as well.

I still think AI shouldn't replace human contact or asking other humans for help, but I'm happy to have another tool in my toolbox.
