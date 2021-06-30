# Discord-plugin-bot2
const Discord = require("discord.js");
const botconfig = require("./botconfig.json");
const tokenfile = require("./tokenfile.json");
const bot = new Discord.Client({disableEveryone: true});
var weather = require('weather-js');
const superagent = require('superagent');
const randomPuppy = require('random-puppy');
var figlet = require("figlet");

let botname = "Chilles bot"

bot.on("ready",async() => {
    console.log(`${bot.user.username} elindult!`)

    let status = [
        "DISCORD BOT NAME", 
        
    ]
    
   setInterval(function() {
       let status = státuszok[Math.floor(Math.random()* státuszok.length)]

       bot.user.setActivity(status, {type: "WATCHING"})
   }, 5000)
})

bot.on("message", async message => {
  let MessageArray = message.content.split(" ");
  let cmd = MessageArray[0];
  let args = MessageArray.slice(1);
  let prefix = botconfig.prefix;

  if(cmd === `${prefix}hello`){
    message.channel.send("```Szia!```")
  }

if(cmd === `${prefix}weather`){
  if(args[0]){
weather.find({search: args.join(" "), degreeType: "C"}, function(err, result) {
  if (err) message.reply(err);
  
  if(result.length === 0){
    message.reply("Kérlek adj meg egy létező település nevet!")
    return; 
  }

 let current = result[0].current;
 let location = result[0].location;

 let WeatherEmbed = new Discord.MessageEmbed()
 .setDescription(`**${current.skytext}**`)
 .setAuthor(`Időjárás itt: ${current.observationpoint}`)                
 .setThumbnail(current.imageUrl)
 .setColor("GREEN")
 .addField("Időzóna:", `UTC${location.timezone}`, true)
 .addField("Fokozat típusa:", `${location.degreetype}`, true)
 .addField("Hőfok:", `${current.temperature}°C`, true)
 .addField("Hőérzet:", `${current.feelslike}°C`, true)
 .addField("Páratartalom:", `${current.humidity}%`, true)

 message.channel.send(WeatherEmbed);
})  

  } else {
    message.reply("Pls give me a capital city name!")  

  } 
} 

if(cmd === `${prefix}cat`){
  let msg = await message.channel.send("**Cat dowloading...**")

  let {body} = await superagent
  .get(`https://aws.random.cat/meow`)

  if(!{body}) return message.channel.send("A file betöltésekor hiba lépett fel!")

  let catEmbed = new Discord.MessageEmbed()
  .setColor("GREEN")
  .addField("Ugye milyen aranyos?", ":3")
  .setImage(body.file)
  .setTimestamp(message.createdAt) 
  .setFooter(botname)

 message.channel.send(catEmbed) 
}





if(cmd === `${prefix}meme`) {
  const subreddits = ["dankmeme", "meme", "memes", "funnymemes"]
  const  random = subreddits[Math.floor(Math.random() * subreddits.length)]

  const IMG = await randomPuppy(random)
  const MemeEmbed = new Discord.MessageEmbed()
  .setColor("ORANGE")
  .setImage(IMG)
