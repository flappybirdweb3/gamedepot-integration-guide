# Introduction


* [What Is GameDepot?](#what-is-gamedepot)
* [Full Documentation](#full-documentation)
* [Quick Overview](#some-methods-are-mandatory-to-be-implemented)
* [Contact Us](#contact-us)

  

**What is GameDepot?**

The goal is to provide a **web3 game infrastructure** based on the TON blockchain including code, smart contracts, libraries, APIs and SDKs so that the web3 game developer community can build codeless (no code required) game applications and release games as quickly as possible for the Telegram user community in general and the TON blockchain community in particular.

GameDepot is a native instant web3 game publishing platform TMA (native Telegram mini App) that helps game developers publish and showcase their games on one of the fastest growing social platforms. With GameDepot toolkit solutions, you can launch Web2 and Web3 games on Telegram and use all the benefits of social and referral mechanisms to increase traffic and revenue.

Games in Game Depot can vary in genre, complexity and gameplay mechanics. They can range from simple arcade-style games to more complex multiplayer experiences. GameDepot games are lightweight and accessible, and users can play them seamlessly within the Telegram app without any downloads or installations.

![cover](https://cdn.dorahacks.io/static/files/19334bc1d48965a96bcd9a94cb2b43be.png)
What Is a Game in GameDepot?
* This is Telegram mini App that runs on top of the Telegram Chat app.
* It can open directly from a Telegram chat, group, or /channel.
* It can be shared directly from the game interface.
* If you already have a game built on HTML5 or WebGL (Unity, Phaser, PixiJS, BabylonJS, Cocos2d, and others), you can easily adapt it to work on our platform.

# Full Documentation

See the [**Wiki**](https://github.com/ton-play/playdeck-integration-guide/wiki) for full documentation, examples, operational details and other information.

---

# Some methods are mandatory to be implemented:

* [**loading()**](https://github.com/ton-play/playdeck-integration-guide/wiki/2.-Integration-guide#4-exchange-of-information-with-the-wrapper) At the start of the game loading process, it's essential to transmit loading (1), and similarly, at the end when the game has finished loading (100). If the loading progress reaching 100% is not signaled, the Play button within the wrapper won't become active.

* The game should consider to use correct user locale for rendering proper UI texts. You can find locale by calling [**getUserProfile()**](https://github.com/ton-play/playdeck-integration-guide/wiki/2.-Integration-guide#1-getting-user-information) method OR use devices locale in order of prioriry: `(navigator.languages && navigator.languages.length) ? navigator.languages[0] : navigator.language;`

* [**setData()/getData()**](https://github.com/ton-play/playdeck-integration-guide/wiki/2.-Integration-guide#2-cloud-save) It can be utilized for cloud saving and cross-device experiences. For example, if your game has levels or your players accumulate some in-game bonuses, you can save and share that information using these methods so that you do not ruin the user experience. This can also be implemented through other methods on the developer side.

---

Inside GameDepot, your game runs in an iFrame in our Wrapper.
The process of passing data between your game and our Wrapper is via `window.postMessage`.
[Window: postMessage() docs](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage)

#### Let's look at an example of listening to messages and processing data from our Wrapper.

```javascript
// Creating an event listener gamedepot
window.addEventListener('message', ({ data }) => {
  if (!data || !data['gamedepot']) return;

  pdData = data['gamedepot'];

  // By default, gamedepot sends "{ gamedepot: { method: "play" }}" after pressing the play button in the gamedepot-menu
  if (pdData.method === 'play') { 
    if (runner.crashed && runner.gameOverPanel) {
      runner.restart();
    } else {
      var e = new KeyboardEvent('keydown', { keyCode: 32, which: 32 });
      document.dispatchEvent(e);
    }
  }
  
  // Getting the gamedepot-menu status, after using the getGameDepotState method
  if (pdData.method === 'getGameDepotState') {
    window.GameDepotIsOpen = data.value; // if true, then the playlist is open
  }
});

const { parent } = window

const payload = {
  gamedepot: {
  method: 'getGameDepotState',
  },
};

// calling the method
parent.postMessage(payload, '*');
```

> In your example, we track the event of pressing the "play" button in the gamedepot-menu, and also get the result of the [getGameDepotState](https://github.com/ton-play/playdeck-integration-guide/wiki/2.-Integration-guide#4-exchange-of-information-with-the-wrapper) method.
> Obviously, you can't call the method directly. We have saved the logic of constructing data for messages.
> For example, you want to use the [`loading`](https://github.com/ton-play/playdeck-integration-guide/wiki/2.-Integration-guide#4-exchange-of-information-with-the-wrapper) method. To do this, you need to create an object with 2 fields: `method`, `value`
> Where the value of the `method` field will be the name of the method to be called, and the `value` field will be the loading state data.

#### Message Example

```javascript
const payload = {
  gamedepot: {
    method: 'loading',
    value: 100,
  },
};

parent.postMessage(payload, '*');
```


You can find usage examples and detailed information on each method in [our guide](https://github.com/ton-play/playdeck-integration-guide/wiki/2.-Integration-guide).

# Contact Us

- Flapper.lol: [Register an account & submit your game]([https://flapper.lol)
- Telegram: [@inotgame](https://t.me/inotgame)
- Website: [gamedepot.meme](https://gamedepot.meme)
- Our game platform: [GameDepot](https://t.me/gamedepotmeme)



[What Is GameDepot?](#what-is-gamedepot)

[Going on TOP](#what-is-gamedepot)

[What Is GameDepot?](#what-is-gamedepot)
