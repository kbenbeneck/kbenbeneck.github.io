---
layout: post
title:      "Final Project Blog Post"
date:       2021-03-19 03:33:15 +0000
permalink:  final_project_blog_post
---


My final project is a board game that’s like CandyLand with a pagan twist. Instead of moving ginger bread guards on a rainbow road to get to King Kandy’s Castle, classical elemental symbols aim to leave impact around an isometric pathway composed of lunar phases, planetary bodies, and astrological symbols. 

Similar to drawing cards with colors or locations like the Peppermint Forest, symbols and moon phases are summoned from a pool. Players move to the next closest moon phase or to the exact position of the astral symbol. 

Upon landing on a spot, that player element leaves an impact symbol on that location and gains +1 impact score. The impact symbol will remain until another player lands on that spot. Before each player moves, the impact symbols on the board are counted and the Scores component updates. Players with the leading score are pushed into a winners array.

The game may end when a player lands on the last position (space 96) and is part of the winners array. If tied, the array’s length is > 1, the player on space 96 is moved to 0 and the game continues until won by a player or the pool runs dry. 

Once the game is won by playerOne, the winning impact score and moves history are fetch(post)ed to a Rails API that allows cross origin requests via the Rack Cors gem.   

The game makes use of the react router, rendering 3 keyboard navigable links to pages or collections of components. Home, explains the game logic. Game, conditionally displays the choose player form or the game board,  if playerOne exists. Lastly, Scores, fetches the winning game stats from previously won games. 

At the heart of this program, the redux middleware places all data in one place, the store. A single source of truth. All state changes are products of dispatching actions to the root reducer and returning new state objects. 

Upon connecting the spaces container to the store and passing it props including each player’s symbol, score, and position, the container may then dispatch actions moving components but to where?

When I designed the moon phase arrangement on the board, I knew that New Moons would be on every 8th index, starting at 0. Waxing Crescents on every 8th starting at 1 and so on… So I numbered each space, 0-96. On each turn, a phase or symbol is randomly selected and passed to whereTo() function along with the players current position. Relative to point A, and the selected symbol, a switch statement returns the next closest position. 







export function whereTo(selection, pointA) {
    function findBMoons(element) {
        return element > pointA
    }
    
    switch(selection) {
        case '🌑':
            return [0,8,16,24,32,40,48,56,64,72,80,88,96].find(findBMoons);
        case '🌒':
            return [1,9,17,25,33,41,49,57,65,73,81,89,96].find(findBMoons);

export function hops(a, b) { 
    let hopForward = arr.filter(function(num) {
        return (num >= a && num <= b)
    })
    let hopBack = arr.filter(function(num) {
       return (num <= a && num >= b) 
    })
    return ( a <= b ) ? hopForward : hopBack.reverse()  
}

let hops1 = () => hops1Arr.map(
                (hop, i) => {
                    setTimeout(() => 
                        this.props.dispatch({ type: 'MOVE_PLAYER_ONE', payload: hop }), i * 25)
                        return console.log(`You (${playerOne}) are moving...`)} )




With point A and point B calculated, all would be player movement hops are filtered into an array. The array elements are then timely mapped, dispatching a ‘MOVE PLAYER’ action every 25 ms. When the position, or space number is passed to the positions function, specific grid row and cell style objects are returned, keeping all components on the path. Each component is styled in line, the result of positions function, relative to props current position. 



export function positions(pos) {
    switch(pos) {
        case 0:
            return {gridColumn:"9",gridRow:"9"};
        case 1:
            return {gridColumn:"9",gridRow:"8"};


  render() {
        const moveTo = positions(this.props.pOneCurrentPosition)
        return (
            <div id="player-one" className={this.props.playerOne} style={moveTo}>



