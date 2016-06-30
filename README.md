# mansa-bot
discord bot
var env = require('../config.json'),
    Help = require('./Help.js'),
    Imgur = require('./Imgur.js'),
    Giphy = require('./Giphy.js'),
    Urban = require('./Urban.js');

var MansaBot = function () {
    this.keywords = env.keywords;
    this.Help = new Help;
    this.Imgur = new Imgur;
    this.Giphy = new Giphy;
    this.Urban = new Urban;
};

MansaBot.prototype.loadKeywords = function ()
{
    var result = [];
    for (var i in this.keywords) {
        if (this.keywords.hasOwnProperty(i)) {
            result.push(this.keywords[i]);
        }
    }
    return result;
}

MansaBot.prototype.checkMessageForKeywords = function(message, triggers, callback)
{
    for(var i = 0; i != triggers.length; i++) {
        var substring = triggers[i];
        if (message.indexOf(substring) == 0) {
            return callback(substring);
        }
    }
    return callback(0);
}

MansaBot.prototype.getKeyByValue = function(object, value)
{
    for(var prop in object) {
        if(object.hasOwnProperty(prop)) {
            if(object[prop] == value)
                return prop;
        }
    }
}

MansaBot.prototype.runKeywordFunction = function(keywordFunction, keyword, message, callback)
{
    this[keywordFunction].Message(keyword, message, callback);
}

module.exports = MansaBot;
