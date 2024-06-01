---
title: സംവേദനക്ഷമത നൽകാം
---

<Intro>

ഉപയോഗിക്കുന്ന ആളുടെ പ്രവർത്തന ഫലമായി സ്‌ക്രീനിൽ ചില മാറ്റങ്ങൾ സംഭവിക്കുന്നു. ഉദാഹരണം ഒരു കൂട്ടം ചിത്രങ്ങളിൽ ഒന്നിൽ ക്ലിക്ക് ചെയ്യുമ്പോൾ അത് സജീവമായി(Active/Selected) കാണിക്കുന്നു. അവസരങ്ങൾക്കനുസരിച് മാറുന്ന ഇത്തരം വിവരങ്ങളെ റിയാക്റ്റിൽ സ്റ്റേറ്റ്(State) എന്ന് വിളിക്കുന്നു. ആവശ്യാനുസരണം സ്റ്റേറ്റിനെ component യിൽ ചേർക്കുകയോ ചേർത്ത സ്റ്റേറ്റിനെ മാറ്റം വരുത്തുകയോ ചെയ്യാം. ഈ അധ്യായത്തിൽ, വിവിധ സാഹചര്യങ്ങളോട് പ്രതികരിക്കാൻ കഴിയുന്ന components എങ്ങനെ എഴുതാമെന്നും അതിൽ സ്റ്റേറ്റ് എങ്ങനെ ചേർക്കാമെന്നും അവയിൽ അനുയോജ്യമായ മാറ്റങ്ങൾ വരുത്തുന്നതെങ്ങനെ എന്നും പഠിക്കാം.

</Intro>

<YouWillLearn isChapter={true}>

* [user തുടക്കമിടുന്ന പ്രവർത്തനങ്ങൾ എങ്ങനെ കൈകാര്യം ചെയ്യും](/learn/responding-to-events)
* [components എങ്ങനെ വിവരങ്ങൾ "ഓർക്കുന്നു"](/learn/state-a-components-memory)
* [react എങ്ങനെ 2 ഭാഗങ്ങളായി UI(യൂസർ ഇന്റർഫേസ്)യിൽ മാറ്റങ്ങൾ വരുത്തുന്നു](/learn/render-and-commit)
* [നിങ്ങൾ മാറ്റിയ ഉടനെ എന്തുകൊണ്ട് സ്റ്റേറ്റ് മാറുന്നില്ല](/learn/state-as-a-snapshot)
* [ഒന്നിന് പിറകെ ഒന്നായി സ്റ്റേറ്റ് മാറ്റങ്ങളെ എങ്ങനെ ക്രമീകരിക്കാം](/learn/queueing-a-series-of-state-updates)
* [State ഇലെ  object ഇൻ എങ്ങനെ മാറ്റം വരുത്താം ](/learn/updating-objects-in-state)
* [State ഇലെ  array ഇൻ എങ്ങനെ മാറ്റം വരുത്താം ](/learn/updating-arrays-in-state)

</YouWillLearn>

## eventകളോട് പ്രതികരിക്കാം {/*responding-to-events*/}

Eventകളെ കൈകാര്യം ചെയ്യാനുള്ള event handlerകളെ JSX ഇൽ കൂട്ടിച്ചേർക്കാൻ react അവസരം നൽകുന്നു. Input elementകളിൽ click ചെയ്യുന്നതും മുകളിലൂടെ(hover) മൗസ് പോയിന്റർ ചലിപ്പിക്കുന്നതും focus ചെയ്യുന്നതും കൈകാര്യം ചെയ്യാൻ കഴിയുന്ന പ്രോഗ്രാമർ ഉണ്ടാക്കുന്ന functions ആണ് event handlerകൾ.

ബ്രൌസർ അടിസ്ഥാനമായാ `<button>` പോലുള്ള Build-in components ബ്രൌസർ അടിസ്ഥാനമായ `onClick` പോലുള്ള events മാത്രമേ പിന്തുണക്കുകയൊള്ളു, എന്നാൽ നമ്മൾ ഉണ്ടാക്കുന്ന componentsഇൽ Props ആയി നൽകുന്ന event handlerകൾക്ക് Applicationന് അനുയോചിയമായ പേരുകൾ നൽകാവുന്നതാണ്.


<Sandpack>

```js
export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert('Playing!')}
      onUploadImage={() => alert('Uploading!')}
    />
  );
}

function Toolbar({ onPlayMovie, onUploadImage }) {
  return (
    <div>
      <Button onClick={onPlayMovie}>
        Play Movie
      </Button>
      <Button onClick={onUploadImage}>
        Upload Image
      </Button>
    </div>
  );
}

function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}
```

```css
button { margin-right: 10px; }
```

</Sandpack>

<LearnMore path="/learn/responding-to-events">

Event  handlerകൾ നൽകുന്നത് എങ്ങനെ എന്ന് പഠിക്കാൻ ഇവിടെ വായിക്കാം **[Responding to Events](/learn/responding-to-events)**.

</LearnMore>

## State: componentഇന്റെ ഓർമ്മ {/*state-a-components-memory*/}

കോംപോണന്റുകൾക്ക് ചിലപ്പോഴൊക്കെ സ്ക്രീനിൽ കാണിക്കുന്നതിൽ മാറ്റം വരുത്തേണ്ടിവരും. ഫോമിൽ ടൈപ്പ് ചെയ്യുമ്പോൾ ഇൻപുട്ട് ഫീൽഡ് അപ്ഡേറ്റ് ആവണം, ഇമേജ് കാരോസെല്ലിൽ 'അടുത്തത്(next)' ക്ലിക്ക് ചെയ്യുമ്പോൾ ചിത്രംമാറ്റണം, 'വാങ്ങുക(buy)' എന്ന ബട്ടൺ ക്ലിക്ക് ചെയ്യുമ്പോൾ ഉൽപ്പന്നം ഷോപ്പിംഗ് കാർട്ടിൽ ചേർക്കണം. ഇപ്പോൾ നൽകിയ മൂല്യം, select ചെയ്ത ചിത്രം, ഷോപ്പിംഗ് കാർട്ടിലെ ഉൽപ്പന്നങ്ങൾ എന്നിവ react componentഇന് ഓർത്തുവെക്കേണ്ടതുണ്ട് . ഈ തരത്തിലുള്ള കോംപോണന്റ്-സ്പെസിഫിക് മെമ്മറിയെ ആണ് *സ്റ്റേറ്റ് (state)* എന്ന് വിളിക്കുന്നത്. 

[`useState`](/reference/react/useState) ഹുക്ക് ഉപയോഗിച്ച് സ്റ്റേറ്റ് ഒരു കോംപോണന്റിൽ ചേർക്കാം. *ഹുക്കുകൾ (Hooks)* പ്രത്യേക ഫംഗ്ഷനുകൾ ആണ്, കോംപോണന്റുകൾക്ക് റിയാക്ട് സവിശേഷതകൾ (സ്റ്റേറ്റ് അതിലെ ഒരു സവിശേഷതയാണ്) ഉപയോഗിക്കാനാവുന്നത് react സവിശേഷതകൾ(സ്റ്റേറ്റ് അതിലെ ഒരു സവിശേഷതയാണ്) componentഇൽ ഉപയോഗിക്കാൻ സഹായിക്കുന്ന പ്രത്യേക ഫംഗ്ഷനുകൾ ആണ് *ഹൂക്കുകൾ (Hooks)*. `useState` ഹുക്ക് നിങ്ങളെ സ്റ്റേറ്റ് വേരിയബിൾ നിർമ്മിക്കാൻ സഹായിക്കുന്നു. useState ആദ്യത്തെ വാല്യൂ(initial value) സ്വീകരിക്കുകയും stateഇന്റെ പേരും മൂല്യമാറ്റത്തിന് സഹായിക്കുന്ന "setter" ഫങ്ക്ഷനും തിരിച് തരുന്നു.

```js
const [index, setIndex] = useState(0);
const [showMore, setShowMore] = useState(false);
```

ഇവിടെ ഒരു ഇമേജ് ഗാലറി ക്ലിക്കിൽ സ്റ്റേറ്റ് എങ്ങനെ ഉപയോഗിക്കുകയും അപ്ഡേറ്റ് ചെയ്യുകയും ചെയ്യുന്നു എന്ന് കാണാം:

<Sandpack>

```js
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const [showMore, setShowMore] = useState(false);
  const hasNext = index < sculptureList.length - 1;

  function handleNextClick() {
    if (hasNext) {
      setIndex(index + 1);
    } else {
      setIndex(0);
    }
  }

  function handleMoreClick() {
    setShowMore(!showMore);
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleNextClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i>
        by {sculpture.artist}
      </h2>
      <h3>
        ({index + 1} of {sculptureList.length})
      </h3>
      <button onClick={handleMoreClick}>
        {showMore ? 'Hide' : 'Show'} details
      </button>
      {showMore && <p>{sculpture.description}</p>}
      <img
        src={sculpture.url}
        alt={sculpture.alt}
      />
    </>
  );
}
```

```js data.js
export const sculptureList = [{
  name: 'Homenaje a la Neurocirugía',
  artist: 'Marta Colvin Andrade',
  description: 'Although Colvin is predominantly known for abstract themes that allude to pre-Hispanic symbols, this gigantic sculpture, an homage to neurosurgery, is one of her most recognizable public art pieces.',
  url: 'https://i.imgur.com/Mx7dA2Y.jpg',
  alt: 'A bronze statue of two crossed hands delicately holding a human brain in their fingertips.'
}, {
  name: 'Floralis Genérica',
  artist: 'Eduardo Catalano',
  description: 'This enormous (75 ft. or 23m) silver flower is located in Buenos Aires. It is designed to move, closing its petals in the evening or when strong winds blow and opening them in the morning.',
  url: 'https://i.imgur.com/ZF6s192m.jpg',
  alt: 'A gigantic metallic flower sculpture with reflective mirror-like petals and strong stamens.'
}, {
  name: 'Eternal Presence',
  artist: 'John Woodrow Wilson',
  description: 'Wilson was known for his preoccupation with equality, social justice, as well as the essential and spiritual qualities of humankind. This massive (7ft. or 2,13m) bronze represents what he described as "a symbolic Black presence infused with a sense of universal humanity."',
  url: 'https://i.imgur.com/aTtVpES.jpg',
  alt: 'The sculpture depicting a human head seems ever-present and solemn. It radiates calm and serenity.'
}, {
  name: 'Moai',
  artist: 'Unknown Artist',
  description: 'Located on the Easter Island, there are 1,000 moai, or extant monumental statues, created by the early Rapa Nui people, which some believe represented deified ancestors.',
  url: 'https://i.imgur.com/RCwLEoQm.jpg',
  alt: 'Three monumental stone busts with the heads that are disproportionately large with somber faces.'
}, {
  name: 'Blue Nana',
  artist: 'Niki de Saint Phalle',
  description: 'The Nanas are triumphant creatures, symbols of femininity and maternity. Initially, Saint Phalle used fabric and found objects for the Nanas, and later on introduced polyester to achieve a more vibrant effect.',
  url: 'https://i.imgur.com/Sd1AgUOm.jpg',
  alt: 'A large mosaic sculpture of a whimsical dancing female figure in a colorful costume emanating joy.'
}, {
  name: 'Ultimate Form',
  artist: 'Barbara Hepworth',
  description: 'This abstract bronze sculpture is a part of The Family of Man series located at Yorkshire Sculpture Park. Hepworth chose not to create literal representations of the world but developed abstract forms inspired by people and landscapes.',
  url: 'https://i.imgur.com/2heNQDcm.jpg',
  alt: 'A tall sculpture made of three elements stacked on each other reminding of a human figure.'
}, {
  name: 'Cavaliere',
  artist: 'Lamidi Olonade Fakeye',
  description: "Descended from four generations of woodcarvers, Fakeye's work blended traditional and contemporary Yoruba themes.",
  url: 'https://i.imgur.com/wIdGuZwm.png',
  alt: 'An intricate wood sculpture of a warrior with a focused face on a horse adorned with patterns.'
}, {
  name: 'Big Bellies',
  artist: 'Alina Szapocznikow',
  description: "Szapocznikow is known for her sculptures of the fragmented body as a metaphor for the fragility and impermanence of youth and beauty. This sculpture depicts two very realistic large bellies stacked on top of each other, each around five feet (1,5m) tall.",
  url: 'https://i.imgur.com/AlHTAdDm.jpg',
  alt: 'The sculpture reminds a cascade of folds, quite different from bellies in classical sculptures.'
}, {
  name: 'Terracotta Army',
  artist: 'Unknown Artist',
  description: 'The Terracotta Army is a collection of terracotta sculptures depicting the armies of Qin Shi Huang, the first Emperor of China. The army consisted of more than 8,000 soldiers, 130 chariots with 520 horses, and 150 cavalry horses.',
  url: 'https://i.imgur.com/HMFmH6m.jpg',
  alt: '12 terracotta sculptures of solemn warriors, each with a unique facial expression and armor.'
}, {
  name: 'Lunar Landscape',
  artist: 'Louise Nevelson',
  description: 'Nevelson was known for scavenging objects from New York City debris, which she would later assemble into monumental constructions. In this one, she used disparate parts like a bedpost, juggling pin, and seat fragment, nailing and gluing them into boxes that reflect the influence of Cubism’s geometric abstraction of space and form.',
  url: 'https://i.imgur.com/rN7hY6om.jpg',
  alt: 'A black matte sculpture where the individual elements are initially indistinguishable.'
}, {
  name: 'Aureole',
  artist: 'Ranjani Shettar',
  description: 'Shettar merges the traditional and the modern, the natural and the industrial. Her art focuses on the relationship between man and nature. Her work was described as compelling both abstractly and figuratively, gravity defying, and a "fine synthesis of unlikely materials."',
  url: 'https://i.imgur.com/okTpbHhm.jpg',
  alt: 'A pale wire-like sculpture mounted on concrete wall and descending on the floor. It appears light.'
}, {
  name: 'Hippos',
  artist: 'Taipei Zoo',
  description: 'The Taipei Zoo commissioned a Hippo Square featuring submerged hippos at play.',
  url: 'https://i.imgur.com/6o5Vuyu.jpg',
  alt: 'A group of bronze hippo sculptures emerging from the sett sidewalk as if they were swimming.'
}];
```

```css
h2 { margin-top: 10px; margin-bottom: 0; }
h3 {
 margin-top: 5px;
 font-weight: normal;
 font-size: 100%;
}
img { width: 120px; height: 120px; }
button {
  display: block;
  margin-top: 10px;
  margin-bottom: 10px;
}
```

</Sandpack>

<LearnMore path="/learn/state-a-components-memory">

മൂല്യം ഓർത്തുവെക്കുന്നതും ഓരോ അവസരത്തിലും എങ്ങനെ മൂല്യമാറ്റം വരുത്താം എന്നും പഠിക്കാൻ **[State: ഒരു componentഇന്റെ ഓർമ്മ](/learn/state-a-components-memory)** വായിക്കുക.

</LearnMore>

## Render ചെയ്യലും commit ചെയ്യലും {/*render-and-commit*/}

നിങ്ങളുടെ componentകൾ സ്ക്രീനിൽ പ്രദർശിപ്പിക്കുന്നതിന് മുമ്പ്, അവ റിയാക്ട് ഉപയോഗിച്ച് റെൻഡർ ചെയ്യപ്പെടണം. ഈ പ്രക്രിയയിലെ ഘട്ടങ്ങൾ മനസ്സിലാക്കുന്നതിലൂടെ നിങ്ങളുടെ കോഡ് എങ്ങനെ പ്രവർത്തിക്കുന്നു എന്നതിനെ കുറിച്ച് നിങ്ങൾക്ക് ചിന്തിക്കാനും അതിന്റെ മാറ്റങ്ങളും പ്രവർത്തനങ്ങളും വിശദീകരിക്കാനും സഹായിക്കും.

നിങ്ങളുടെ componentകൾ അടുക്കളയിൽ രുചികരമായ വിഭവങ്ങൾ തയാറാക്കുന്ന പാചകരാണെന്ന് വിചാരിക്കുക. React ഉപഭോക്താക്കളിൽ നിന്ന് ഓർഡറുകൾ സ്വീകരിക്കുന്ന വെയ്റ്ററും. ഈ UI(User interface) അഭ്യർത്ഥനയും സേവനവും മൂന്ന് ഘട്ടങ്ങളിലായാണ് നടക്കുന്നത്:

1. റെൻഡർ **ട്രിഗർ**  ചെയ്യുക (ഉപഭോക്താക്കളിൽ നിന്ന് ഓർഡർ സ്വീകരിച് അടുക്കളയിൽ എത്തിക്കുക)
2. Component **റെൻഡർ** ചെയ്യുക (വിഭവങ്ങൾ തയാറാക്കുക)
3. DOM-ലേക്ക് **കമ്മിറ്റ്** ചെയ്യുക (ഓർഡർ മേശയിൽ എത്തിക്കുക)

<IllustrationBlock sequential>
  <Illustration caption="ട്രിഗർ" alt="React as a server in a restaurant, fetching orders from the users and delivering them to the Component Kitchen." src="/images/docs/illustrations/i_render-and-commit1.png" />
  <Illustration caption="റെൻഡർ" alt="The Card Chef gives React a fresh Card component." src="/images/docs/illustrations/i_render-and-commit2.png" />
  <Illustration caption="കമ്മിറ്റ്" alt="React delivers the Card to the user at their table." src="/images/docs/illustrations/i_render-and-commit3.png" />
</IllustrationBlock>

<LearnMore path="/learn/render-and-commit">

UI (User interface) ഇലെ മാറ്റങ്ങളും അവയുടെ ജീവിത ചക്രവും(ലൈഫ് സൈക്കിൾ) മനസ്സിലാക്കാൻ  **[റെൻഡർ ചെയ്യലും കമ്മിറ്റ് ചെയ്യലും](/learn/render-and-commit)** എന്ന ഭാഗം വായിക്കുക.

</LearnMore>

## State: വേഗത്തിൽ എടുക്കുന്ന ഫോട്ടോ പോലെ {/*state-as-a-snapshot*/}

സാധാരണ ജാവാസ്ക്രിപ്റ്റ് വേരിയബിളുകളിൽ നിന്നും വ്യത്യാസമായി, റിയാക്ട് സ്റ്റേറ്റ് ഒരു വേഗത്തിൽ എടുത്ത ഫോട്ടോ പോലെ ആണ്. അതിലെ മൂല്യം മാറുന്നത് പെട്ടെന്നു പ്രതിഫലിക്കുന്നില്ല, പകരം അത് ഒരു റീ-റെൻഡർ ട്രിഗർ ചെയ്യുന്നു. ഇതൊരല്പം ആശ്ചര്യപ്പെടുത്തുന്ന കാര്യമാണ്.

```js
console.log(count);  // 0
setCount(count + 1); // Request a re-render with 1
console.log(count);  // Still 0!
```

ഇങ്ങനെ ചെയ്യുന്നതിലൂടെ ചില സങ്കീർണ്ണമായ പ്രശ്നങ്ങൾ മറികടക്കാൻ സഹായകമാകുന്നു. നമുക്ക് ഒരു മെസ്സേജിങ് ആപ്പിന്റെ പ്രവർത്തനം നോക്കാം. ഇവിടെ "Send" എന്ന ബട്ടൺ ക്ലിക്ക് ചെയ്ത് പെട്ടെന്ന് "Bob" എന്ന് മാറ്റുമ്പോൾ എന്താണ് സംഭവിക്കാൻ പോകുന്നതെന്ന് ഊഹിക്കാൻ ശ്രമിക്കു. 5 നിമിഷങ്ങൾക് ശേഷം തുറന്ന് വരുന്ന `alert` ബോക്സിൽ ഏത് പേരാണ് വരുന്നത്?

<Sandpack>

```js
import { useState } from 'react';

export default function Form() {
  const [to, setTo] = useState('Alice');
  const [message, setMessage] = useState('Hello');

  function handleSubmit(e) {
    e.preventDefault();
    setTimeout(() => {
      alert(`You said ${message} to ${to}`);
    }, 5000);
  }

  return (
    <form onSubmit={handleSubmit}>
      <label>
        To:{' '}
        <select
          value={to}
          onChange={e => setTo(e.target.value)}>
          <option value="Alice">Alice</option>
          <option value="Bob">Bob</option>
        </select>
      </label>
      <textarea
        placeholder="Message"
        value={message}
        onChange={e => setMessage(e.target.value)}
      />
      <button type="submit">Send</button>
    </form>
  );
}
```

```css
label, textarea { margin-bottom: 10px; display: block; }
```

</Sandpack>


<LearnMore path="/learn/state-as-a-snapshot">

മൂല്യം മാറ്റിയിട്ടും സ്റ്റേറ്റ് മാറിയതായി തോന്നാത്തത് എന്ത് കൊണ്ടാണെന്ന് മനസിലാക്കാൻ **[State: വേഗത്തിൽ എടുക്കുന്ന ഫോട്ടോ പോലെ](/learn/state-as-a-snapshot)** എന്ന ഭാഗം വായിക്കുക.

</LearnMore>

## ഒന്നിന് പിറകെ ഒന്നായി വരുന്ന ഒരുകൂട്ടം സ്റ്റേറ്റ് അപ്ഡേറ്റുകൾ {/*queueing-a-series-of-state-updates*/}

ഈ componentൽ ഒരു പ്രശ്നമുണ്ട്: '+3' ക്ലിക്ക് ചെയ്താൽ സ്കോർ ഒരു തവണ മാത്രമേ വർദ്ധിക്കുന്നൊള്ളു.

<Sandpack>

```js
import { useState } from 'react';

export default function Counter() {
  const [score, setScore] = useState(0);

  function increment() {
    setScore(score + 1);
  }

  return (
    <>
      <button onClick={() => increment()}>+1</button>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <h1>Score: {score}</h1>
    </>
  )
}
```

```css
button { display: inline-block; margin: 10px; font-size: 20px; }
```

</Sandpack>

[State as a Snapshot](/learn/state-as-a-snapshot) explains why this is happening. Setting state requests a new re-render, but does not change it in the already running code. So `score` continues to be `0` right after you call `setScore(score + 1)`.

```js
console.log(score);  // 0
setScore(score + 1); // setScore(0 + 1);
console.log(score);  // 0
setScore(score + 1); // setScore(0 + 1);
console.log(score);  // 0
setScore(score + 1); // setScore(0 + 1);
console.log(score);  // 0
```

You can fix this by passing an *updater function* when setting state. Notice how replacing `setScore(score + 1)` with `setScore(s => s + 1)` fixes the "+3" button. This lets you queue multiple state updates.

<Sandpack>

```js
import { useState } from 'react';

export default function Counter() {
  const [score, setScore] = useState(0);

  function increment() {
    setScore(s => s + 1);
  }

  return (
    <>
      <button onClick={() => increment()}>+1</button>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <h1>Score: {score}</h1>
    </>
  )
}
```

```css
button { display: inline-block; margin: 10px; font-size: 20px; }
```

</Sandpack>

<LearnMore path="/learn/queueing-a-series-of-state-updates">

Read **[Queueing a Series of State Updates](/learn/queueing-a-series-of-state-updates)** to learn how to queue a sequence of state updates.

</LearnMore>

## Updating objects in state {/*updating-objects-in-state*/}

State can hold any kind of JavaScript value, including objects. But you shouldn't change objects and arrays that you hold in the React state directly. Instead, when you want to update an object and array, you need to create a new one (or make a copy of an existing one), and then update the state to use that copy.

Usually, you will use the `...` spread syntax to copy objects and arrays that you want to change. For example, updating a nested object could look like this:

<Sandpack>

```js
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    name: 'Niki de Saint Phalle',
    artwork: {
      title: 'Blue Nana',
      city: 'Hamburg',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleNameChange(e) {
    setPerson({
      ...person,
      name: e.target.value
    });
  }

  function handleTitleChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        title: e.target.value
      }
    });
  }

  function handleCityChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        city: e.target.value
      }
    });
  }

  function handleImageChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        image: e.target.value
      }
    });
  }

  return (
    <>
      <label>
        Name:
        <input
          value={person.name}
          onChange={handleNameChange}
        />
      </label>
      <label>
        Title:
        <input
          value={person.artwork.title}
          onChange={handleTitleChange}
        />
      </label>
      <label>
        City:
        <input
          value={person.artwork.city}
          onChange={handleCityChange}
        />
      </label>
      <label>
        Image:
        <input
          value={person.artwork.image}
          onChange={handleImageChange}
        />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {' by '}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>
      <img
        src={person.artwork.image}
        alt={person.artwork.title}
      />
    </>
  );
}
```

```css
label { display: block; }
input { margin-left: 5px; margin-bottom: 5px; }
img { width: 200px; height: 200px; }
```

</Sandpack>

If copying objects in code gets tedious, you can use a library like [Immer](https://github.com/immerjs/use-immer) to reduce repetitive code:

<Sandpack>

```js
import { useImmer } from 'use-immer';

export default function Form() {
  const [person, updatePerson] = useImmer({
    name: 'Niki de Saint Phalle',
    artwork: {
      title: 'Blue Nana',
      city: 'Hamburg',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleNameChange(e) {
    updatePerson(draft => {
      draft.name = e.target.value;
    });
  }

  function handleTitleChange(e) {
    updatePerson(draft => {
      draft.artwork.title = e.target.value;
    });
  }

  function handleCityChange(e) {
    updatePerson(draft => {
      draft.artwork.city = e.target.value;
    });
  }

  function handleImageChange(e) {
    updatePerson(draft => {
      draft.artwork.image = e.target.value;
    });
  }

  return (
    <>
      <label>
        Name:
        <input
          value={person.name}
          onChange={handleNameChange}
        />
      </label>
      <label>
        Title:
        <input
          value={person.artwork.title}
          onChange={handleTitleChange}
        />
      </label>
      <label>
        City:
        <input
          value={person.artwork.city}
          onChange={handleCityChange}
        />
      </label>
      <label>
        Image:
        <input
          value={person.artwork.image}
          onChange={handleImageChange}
        />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {' by '}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>
      <img
        src={person.artwork.image}
        alt={person.artwork.title}
      />
    </>
  );
}
```

```json package.json
{
  "dependencies": {
    "immer": "1.7.3",
    "react": "latest",
    "react-dom": "latest",
    "react-scripts": "latest",
    "use-immer": "0.5.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

```css
label { display: block; }
input { margin-left: 5px; margin-bottom: 5px; }
img { width: 200px; height: 200px; }
```

</Sandpack>

<LearnMore path="/learn/updating-objects-in-state">

Read **[Updating Objects in State](/learn/updating-objects-in-state)** to learn how to update objects correctly.

</LearnMore>

## Updating arrays in state {/*updating-arrays-in-state*/}

Arrays are another type of mutable JavaScript objects you can store in state and should treat as read-only. Just like with objects, when you want to update an array stored in state, you need to create a new one (or make a copy of an existing one), and then set state to use the new array:

<Sandpack>

```js
import { useState } from 'react';

let nextId = 3;
const initialList = [
  { id: 0, title: 'Big Bellies', seen: false },
  { id: 1, title: 'Lunar Landscape', seen: false },
  { id: 2, title: 'Terracotta Army', seen: true },
];

export default function BucketList() {
  const [list, setList] = useState(
    initialList
  );

  function handleToggle(artworkId, nextSeen) {
    setList(list.map(artwork => {
      if (artwork.id === artworkId) {
        return { ...artwork, seen: nextSeen };
      } else {
        return artwork;
      }
    }));
  }

  return (
    <>
      <h1>Art Bucket List</h1>
      <h2>My list of art to see:</h2>
      <ItemList
        artworks={list}
        onToggle={handleToggle} />
    </>
  );
}

function ItemList({ artworks, onToggle }) {
  return (
    <ul>
      {artworks.map(artwork => (
        <li key={artwork.id}>
          <label>
            <input
              type="checkbox"
              checked={artwork.seen}
              onChange={e => {
                onToggle(
                  artwork.id,
                  e.target.checked
                );
              }}
            />
            {artwork.title}
          </label>
        </li>
      ))}
    </ul>
  );
}
```

</Sandpack>

If copying arrays in code gets tedious, you can use a library like [Immer](https://github.com/immerjs/use-immer) to reduce repetitive code:

<Sandpack>

```js
import { useState } from 'react';
import { useImmer } from 'use-immer';

let nextId = 3;
const initialList = [
  { id: 0, title: 'Big Bellies', seen: false },
  { id: 1, title: 'Lunar Landscape', seen: false },
  { id: 2, title: 'Terracotta Army', seen: true },
];

export default function BucketList() {
  const [list, updateList] = useImmer(initialList);

  function handleToggle(artworkId, nextSeen) {
    updateList(draft => {
      const artwork = draft.find(a =>
        a.id === artworkId
      );
      artwork.seen = nextSeen;
    });
  }

  return (
    <>
      <h1>Art Bucket List</h1>
      <h2>My list of art to see:</h2>
      <ItemList
        artworks={list}
        onToggle={handleToggle} />
    </>
  );
}

function ItemList({ artworks, onToggle }) {
  return (
    <ul>
      {artworks.map(artwork => (
        <li key={artwork.id}>
          <label>
            <input
              type="checkbox"
              checked={artwork.seen}
              onChange={e => {
                onToggle(
                  artwork.id,
                  e.target.checked
                );
              }}
            />
            {artwork.title}
          </label>
        </li>
      ))}
    </ul>
  );
}
```

```json package.json
{
  "dependencies": {
    "immer": "1.7.3",
    "react": "latest",
    "react-dom": "latest",
    "react-scripts": "latest",
    "use-immer": "0.5.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

</Sandpack>

<LearnMore path="/learn/updating-arrays-in-state">

Read **[Updating Arrays in State](/learn/updating-arrays-in-state)** to learn how to update arrays correctly.

</LearnMore>

## What's next? {/*whats-next*/}

Head over to [Responding to Events](/learn/responding-to-events) to start reading this chapter page by page!

Or, if you're already familiar with these topics, why not read about [Managing State](/learn/managing-state)?
