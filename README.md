# CIT281 Project 4

## Learning Objectives
<li>Gain experience interpreting functional descriptions and specifications to complete an assignment</li>
<li>Gain experience breaking a project into manageable components</li>
<li>Gain experience writing and executing non-web server Node.js JavaScript code using VSCode</li>
<li>Practice creating and using code modules</li>
<li>Practice using modern JavaScript syntax</li>
<li>Gain experience writing and executing Node.js REST API server using VSCode</li>
<li>Gain experience using Fastify with the GET verb, routes, and route parameters</li>
<li>Gain experience working with static data</li>
<li>Gain experience testing code module without using a web server</li>
<li>Gain experience using Postman to test web server routes</li>
<li>Gain experience working with JSON</li>
<li>Extra credit: Gain experience using Fastify with POST, PUT, and DELETE verbs</li>

## Code:

#### p4-data.js
<code>const data = [
{
question: "Q1",
answer: "A1",
},
{
question: "Q2",
answer: "A2",
},
{
question: "Q3",
answer: "A3",
},
];
module.exports = {
data,
};</code>

#### p4-module.js
const fs = require("fs");
const fastify = require("fastify")();
const { data } = require("./p4-data.js");
<br></br>
//getQuestions()
<br></br>
function getQuestions() {
return data.map (item => item.question);
}
console.log(getQuestions());
<br></br>
//getAnswered()
<br></br>
function getAnswers() {
    return data.map (item => item.answer);
}
console.log(getAnswers());
<br></br>
//getQuestionAnswers()
<br></br>
function getQuestionsAnswers(number = "") {
    return data;
    }
    console.log(data);
<br></br>
//getQuestion(number = "")
<br></br>
function getQuestion(number = "") {
    const index = number - 1;
    if (index >= 0 && index < data.length) {
      return data[index].question;
    }
    return null;
  }
  console.log(getQuestion(1));
<br></br>
//getAnswer(number = "")
<br></br>
function getAnswer(number = "") {
    const index = number - 1;
    if (index >= 0 && index < data.length) {
      return data[index].answer;
    }
    return null;
  }
  console.log(getAnswer(1));
<br></br>
//getQuestionAnswer(number = "")
<br></br>
function getQuestionAnswer(number = "") {
    const index = number - 1;
    if (index >= 0 && index < data.length) {
      return {
        question: data[index].question,
        answer: data[index].answer,
        number: data[index].number,
        error: ''
      };
    }
    return null;
  }
  console.log(getQuestionAnswer(1));
<br></br>
/*****************************
  Module function testing
******************************/
<br></br>
function testing(category, ...args) {
    console.log(`\n** Testing ${category} **`);
    console.log("-------------------------------");
    for (const o of args) {
      console.log(`-> ${category}${o.d}:`);
      console.log(o.f);
    }
  }
  <br></br>
  // Set a constant to true to test the appropriate function
  const testGetQs = false;
  const testGetAs = false;
  const testGetQsAs = false;
  const testGetQ = false;
  const testGetA = false;
  const testGetQA = false;
  const testAdd = false;      // Extra credit
  const testUpdate = false;   // Extra credit
  const testDelete = false;   // Extra credit
<br></br>
  // getQuestions()
if (testGetQs) {
    testing("getQuestions", { d: "()", f: getQuestions() });
  }
  <br></br>
  // getAnswers()
  if (testGetAs) {
    testing("getAnswers", { d: "()", f: getAnswers() });
  }
  <br></br>
  // getQuestionsAnswers()
  if (testGetQsAs) {
    testing("getQuestionsAnswers", { d: "()", f: getQuestionsAnswers() });
  }
  <br></br>
  // getQuestion()
  if (testGetQ) {
    testing(
      "getQuestion",
      { d: "()", f: getQuestion() },      // Extra credit: +1
      { d: "(0)", f: getQuestion(0) },    // Extra credit: +1
      { d: "(1)", f: getQuestion(1) },
      { d: "(4)", f: getQuestion(4) }     // Extra credit: +1
    );
  }
  <br></br>
  // getAnswer()
  if (testGetA) {
    testing(
      "getAnswer",
      { d: "()", f: getAnswer() },        // Extra credit: +1
      { d: "(0)", f: getAnswer(0) },      // Extra credit: +1
      { d: "(1)", f: getAnswer(1) },
      { d: "(4)", f: getAnswer(4) }       // Extra credit: +1
    );
  }
  <br></br>
  // getQuestionAnswer()
  if (testGetQA) {
    testing(
      "getQuestionAnswer",
      { d: "()", f: getQuestionAnswer() },    // Extra credit: +1
      { d: "(0)", f: getQuestionAnswer(0) },  // Extra credit: +1
      { d: "(1)", f: getQuestionAnswer(1) },
      { d: "(4)", f: getQuestionAnswer(4) }   // Extra credit: +1
    );
  }
<br></br>
//p4-server.js exports for REST API

  module.exports = {
    getQuestions,
  };

  module.exports = {
    getAnswers,
  };
  
  module.exports = {
    getQuestionsAnswers,
  };

  module.exports = {
    getQuestion,
  };

  module.exports = {
    getAnswer,
  };

  module.exports = {
    getQuestionAnswer,
  };
  
#### p4-server.js

onst fs = require("fs");
const fastify = require("fastify")();
const { getQuestions, 
        getAnswers, 
        getQuestionsAnswers, 
        getQuestion, 
        getAnswer, 
        getQuestionAnswer } = require("./p4-module.js");

//Route: /cit/question

fastify.get("/cit/question", (request, reply) => {
    const question = getQuestion();
reply 
.code(200)
.header("Content-Type", "application/json; charset=utf-8")
.send({error: '', statusCode: 200, question});
});

//Route: /cit/answer

fastify.get("/cit/answer", (request, reply) => {
    const answer = getAnswer();
reply 
.code(200)
.header("Content-Type", "application/json; charset=utf-8")
.send({error: '', statusCode: 200, answer});
});

//Route: /cit/questionanswer

fastify.get("/cit/questionanswer", (request, reply) => {
    const questionsanswer = getQuestionAnswer();
reply 
.code(200)
.header("Content-Type", "application/json; charset=utf-8")
.send({error: '', statusCode: 200, questionanswer});
});

//Route: /cit/question/:number

fastify.get("/cit/question/:number", (request, reply) => {
    const question = getQuestion();
    const {number} = index[data];
reply 
.code(200)
.header("Content-Type", "application/json; charset=utf-8")
.send({error: '', statusCode: 200, question, number});
});

//Route: /cit/answer/:number

fastify.get("/cit/answer/:number", (request, reply) => {
    const answer = getAnswer();
    const {number} = index[data];
reply 
.code(200)
.header("Content-Type", "application/json; charset=utf-8")
.send({error: '', statusCode: 200, answer, number});
});

//Route: /cit/questionanswer/:number

fastify.get("/cit/questionanswer/:number", (request, reply) => {
    const questionanswer = getQuestionAnswer();
    const {number} = index[data];
reply 
.code(200)
.header("Content-Type", "application/json; charset=utf-8")
.send({error: '', statusCode: 200, questionanswer, number});
});

//Route: *

fastify.get('*', (request, reply) => {
reply 
.code(404)
.header("Content-Type", "application/json; charset=utf-8")
.send({error: 'Route not found', statusCode: 404 });
});


// Start server and listen to requests using Fastify
const listenIP = "localhost";
const listenPort = 8082;
fastify.listen(listenPort, listenIP, (err, address) => {
if (err) {
console.log(err);
process.exit(1);
}
console.log(`Server listening on ${address}`);
});



