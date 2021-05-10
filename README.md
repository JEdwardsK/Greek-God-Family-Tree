# 🔱 Pantheon - Greek Mythology Family Tree Database 🔱
> SEI Project 2

This project was a 48 hour pair coding assignment with the objective of consuming an external API and displaying the data in a React App.

My partner for the project was Sami Hakim  [<img src="./src/assets/images/github.png" width="25">](https://github.com/Hamisakim) [<img src="./src/assets/images/linkedin.png" width="30">](https://www.linkedin.com/in/samihakim/)

Click the image below to navigate to the deployed app

[<img width="1423" alt="Screenshot 2021-05-07 at 14 19 11" src="https://user-images.githubusercontent.com/76621344/117462809-ec2ee580-af46-11eb-87d2-46876b4a199e.png">](https://thepantheon.netlify.app/)

## Installing / Getting Started

The initial setup for the front end is handled by `create-react-app` using the GA London template
```shell
$ yarn create react-app client --template cra-template-ga-ldn
```
Packages used
- axios
- bulma
- react-router-dom

When forking/cloning the repo, a yarn and yarn start should start up the app

```shell
$ yarn; yarn start
```

## Development

### Idea Stage

We started by looking for a free API to work with. We found that there tends to be an API for everything you can think of. We moved from a space theme to something around greek mythology. Initially we found [greekmyths](https://greekmythology1.docs.apiary.io/), but we couldn't get it to work. We settled on [GreekAPI](https://github.com/newsh/GreekAPI), which had less information per character, but more characters to select.

We first tested our routes through Insomnia to visualise the request responses

### Dev Stage

Rather than having to deal with merge conflicts and duplicating work, we pair coded on Sami's repo through a combination of LiveShare and screen sharing on Zoom.

#### Highlights

I used this project as an opportunity to practice creating images using pure CSS, implementing this in the HomePage component.

All of the elements were wrapped in a container and sized through absolute positioning and percentages, in an attempt to make it scalable. a snippet below shows the HTML for the sword and shield

```HTML
<div className="sword-and-shield">
   <div className="sword-blade"></div>
   <div className="sword-hilt"></div>
   <div className="sword-guard"></div>
   <div className="sword-shield"></div>
   <div className="sword-end"></div>
 </div>
</div>
```
and the styling
```scss
$bronze: #d27a25;
$steel: grey;
$shield: rgb(110, 67, 14);

.sword-and-shield {
 position: absolute;
 height: 65%;
 width: 32.5%;
 // border: 2px solid red;
 top: 45%;
 left: 45%;
 z-index: 10;
}
.sword-blade {
 position: absolute;
 height: 60%;
 top: 30%;
 width: 10%;
 left: 45%;
 background-color: $steel;
}
.sword-hilt {
 position: absolute;
 height: 10%;
 width: 6%;
 background: repeating-linear-gradient(-45deg, #920027, #920027 5px, black 5px, black 10px);
 top: 19%;
 left: 47%;
}
.sword-guard {
 position: absolute;
 height: 3%;
 width: 25%;
 background-color: $bronze;
 left: 38%;
 top: 29%;
 border: 3px outset $shield;
}

.sword-end {
 position: absolute;
 background-color: $bronze;
 border-radius: 50%;
 top: 14%;
 height: 8%;
 width: 10%;
 left: 43%;
 border: 7px inset $shield;
}
.sword-shield {
 position: absolute;
 background: radial-gradient(circle, transparent, $shield),
   repeating-conic-gradient(from 0deg, #ff8a00 0deg 15deg, black 15deg 30deg);
 border: 10px outset $bronze;
 border-radius: 50%;
 top: 50%;
 height: 45%;
 width: 45%;
 left: 35%;
}
```
I wanted to have some vines to wrap the pillar or frame the page, but couldn't work out how to implement it. Additionally, I had to cheat with the whitespace in between the handle and the vase stem, which varies between passable and glaringly obvious.

## Learning Outcomes

### Wins/ Challenges

This was a great opportunity to experience pair coding, using a combination of LiveShare in VSCode, Slack and Zoom aided in this, as well as gaining more familiarity with these technologies.

It was beneficial to work through problems together and be able to split tasks up to work independently.

Another win was gaining more confidence with styling and React Hooks.

With regards to challenges, we came across a problem with data structure in the API. For some reason the husband or wife key was an array
```json
"wife": [
 {
   "personID": 65,
   "name": "Aphrodite"
 }
],
```
Perhaps to account for if someone had multiple spouses? Jocasta has two Husbands, her first, Laius, and her second, Oedipus. However her data only accounts for her most recent, not her former...

We also had to use conditional rendering depending on whether the person in particular had someone in the specified relationship.

### Future Improvements
#### Added Information for Characters

The API we used was limited in the data provided, ideally we would have had things like bio pages and images etc. However due to the number of characters (circa 200), it would be impractical to manually seed all data for each one, especially given the time constraint.

We considered linking to a wikipedia page or another wiki possibly through an inline frame element that would appear on hover, however coming up with a method to do so was problematic due to the varied naming conventions of mythological characters.

Linking could work for example like
```javascript
<Link to={`https://en.wikipedia.org/wiki/${godData.name}`}>{godData.name}</Link>
```

#### Fixing Random
We have a Random button on the nav bar that links to a random family tree. for testing we simply put in Uranus for presentation
```javascript
<div className='navbar-start' onClick={getRandomNumber()}>
 <Link to='/family-tree/Uranus'
   className='navbar-item'>
     Random
 </Link>
</div>
```

We actually need a list of all of the characters in the database in an array to select one from random. We prepared a function to handle the index number.

```javascript
export const getRandomNumber = () => {
 const randomIndex = Math.floor(Math.random() * 220)
 console.log(randomIndex)
 return randomIndex
}
```
We would then have to make a request to the API and map through to get the array. The  `getRandomNumber()` function would be rewritten to replace 220 with `namesArray.length`. A defined variable could then be placed in the link. It would probably involve setting some variables to state, and using the `useHistory()` hook, if the `<Link>` doesn't work.
```javascript
//? either Link or useHistory()

const [randomName, setRandomName] = useState('')

// const history = useHistory()
const getRandomName = () => {
 const nameFromArray = namesArray[getRandomNumber()]
 setRandomName(nameFromArray)
 // history.push(`/family-tree/${randomName}`)
}


return (
 <div className='navbar-start' onClick={getRandomNumber()}>
<Link to={`/family-tree/${randomName}`}
 className='navbar-item'>
   Random
</Link>
</div>
)
```

# Walkthrough
From the navbar, users can navigate to view all gods.

<figure>
<img width="1440" alt="Screenshot 2021-05-07 at 15 17 32" src="https://user-images.githubusercontent.com/76621344/117463233-59427b00-af47-11eb-92ca-a93ef305deb1.png">
<figcaption styles="color: #00ff00">Homepage</figcaption>
</figure>
From here they can scroll to search, or optionally use the search function.

<figure>
 <img width="1440" alt="Screenshot 2021-05-07 at 15 18 14" src="https://user-images.githubusercontent.com/76621344/117463344-7414ef80-af47-11eb-8ae8-43584b286eaa.png">
 <figcaption><small>Gods Index Page</small></figcaption>
</figure>

After selecting they will be taken to the Family Tree. Oedipus is an amusing example as his father is not the father of his siblings, despite how the tree presents the data.
<figure>
 <img width="1440" alt="Family tree for Oedipus" src="./src/assets/images/oedipus.png">
 <figcaption><small>Gods Show Page</small></figcaption>
</figure>
From here users can click any character name on the tree to be taken to that family member’s tree.
<figure>
 <img width="1440" alt="Family tree for Oedipus' mother-wife, Jocasta" src="./src/assets/images/jocasta.png">
 <figcaption><small>Gods Show Page Family tree for Oedipus' mother-wife, Jocasta</small></figcaption>
</figure>

Clicking on the navbar elements return either to
- the Homepage (🔱)
- the Index (All Gods)
- Uranus (Random)
