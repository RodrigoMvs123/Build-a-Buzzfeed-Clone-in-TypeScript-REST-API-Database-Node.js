# Build-a-Buzzfeed-Clone-in-TypeScript-REST-API-Database-Node.js

https://www.youtube.com/watch?v=Ubd-a8_-G8s

App.tsx
import React, {useState, useEffect, createRaf } from 'react'
import Title from './Components/Title'
import QuestionsBlock from "./components/QuestionBlock"
import { AnswerBlock } from './Interfaces'
import {QuizData, Content} from '../interfaces'

const App() = () => {

  const [quiz, setQuiz] = useState <QuizData | null> ()
  const [chosenAnswerItems, setChosenAnswerItems] = useState<string>[>({})]
  const [unansweredQuestionIds, setUnansweredQuestionIds]= useState<number[] | undefined>([])
  const [showAnswere, setShowAnswere] = useState <boolean>(false)

  type ReduceType = {
    id?: {}
  }
  const refs = unansweredQuestionIds?.reduce<ReduceType | any> ((acc, id) => {
    acc[id as unknown as keyof Reduce Type] = createRef<HTMLDiveElement | null>()
    return acc 
  }, {})

  const answerRef = createRef <HTMLDiveElement | null>()

  const fechData = async () => {
    try {
    const reponse = await fech(input: 'https://localhost:8000/quiz-item')
    const json = await response.json()
    setQuiz(json)
    } catch (err) {
      console.error(err) 
    }
  }

useEffect ( () => {
  fetchData()

}, []) 

use useEffect(() => {
  const unanswereIds  = quiz?.content?.map(({id } : Content) => id)
  setUnansweredQuestionIds(unanswereIds)

}, [quiz])

console.log(unansweredQuestionIds)

useEffect(() => {
  if(chosenItems.length > 0 && unansweredQuestionIds) {  

    if (showAnswer && answerRef.current) {
       answerRef.current.scrollIntoView ({behavior: 'smooth'})
    }
    if (unansweredQuestionIds.length <= 0 && chosenAnswerItems.length >= 1 ) {
      setShowAnswere(true)
      
    } else {
      const highestId = Math.min(...unansweredQuestionIds)
      refs[highestId].current.acrollIntoView ({behavior: 'smooth'})
    }

  }
  
}, [unansweredQuestionIds,  chosenAnswerItems.length, showAnswer, answerRef.current, refs])

  return (
    <div className="app">
      <Title title = {quiz?.title} subtitle={quiz?subtitle}/>
      {refs && quiz? content.map ((content:Content) => (
        <QuestionsBlock
        key={content.id}
        quizItem= {content}
        chosenAnswerItems={chosenAnswerItems}
        chosenAnswerItems={chosenAnswerItems}
        setChosenAnswerItems={setChosenAnswerItems}
        unansweredQuestionIds={unansweredQuestionIds}
        setUnansweredQuestionIds={setUnansweredQuestionIds}
        ref={refs[content.id]}
         />
        
      ))}

      {showAnswere && 
      <AnswerBlock
         answerOptions= {quiz?.answers }
         chosenAnswers={chosenAnswerItems}
         ref={answerRef}
         />}


    </div>
  )
}

export default App

Index.css
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');

* {
    font-family: 'Montserrat', sans-serif;
}

body { 
    display: flex;
    justify-content: center;
}

.app {
    width: 37.5rem;

}

h1 {
    font-size: 2.5rem;
    line-height: 1.05;
    text-align: left;
}

h2 {
    font-size: 3.5rem;
    line-height: 1.05;
    text-align: center;
}

h3 {
    margin: 0,625rem;
}

p {
    font-size: 1.125rem;
    line-height: 1.2;
}

.title-block {
    width: 37.5rem;
    height: 13.125rem;
    background-color: rgb(102, 69, 221);
    border-radius: 0.313rem;
    display: flex;
    justify-content: center;
    align-items: center;
    color: rgb(255, 255, 255);
}

.questions-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}

.question-block {
    width: 17.875rem;
    overflow: hidden;
    background-color: rgb(255, 255, 255);
    border-radius: 0.313rem;
    box-shadow: rgba(0, 0, 0, 0.7); 0 0 0 0.063rem;
    text-align: center;
    margin-bottom: 0.938rem;
    border: none;
    padding: 0;

}

.question-block p {
    font-size: 0.8rem;
    font-style: italic;
}

.question-block a {
    color: rgb(135, 135, 135);
    text-decoration: none;

}

.answer-block {
    width: 36.5rem;
    background-color: rgb(255, 61, 158);
    border-radius: 0.313rem;
    display: flex;
    align-items: center;
    flex-direction: column;
    color: rgb(255, 255, 255);

}

.answer-block img {
    width: 90%;

}


QuestionBlock.tsx
import React from 'react'
import {Question} from '../../interfaces'

const QuestionBlock = ({
  quizItemId,
  question,
  chosenAnswerItems,
  setChosenAnswerItems,
  unansweredQuestionIds,
  setUnansweredQuestionIds
}: {  
  quizItemId: number;
  question:Question,
  chosenAnswerItems: string[],
  setChosenAnswerItems: function,
  unansweredQuestionIds: number[] | undefined,
  setUnansweredQuestionIds: function 
}) => {


const handleClick = () => {
  setChosenAnswerItems ((prevState: string[]) => [...prevState, question.text])
  setUnansweredQuestionIds(unansweredQuestionIds?.filter((id: number) => id!== quizItemId ))
}

   const validPick = ! chosenAnswerItems?.includes(question.text) && 
   ! unansweredQuestionIds?.includes(quizItemId)

  return (
   <button 
      className="question-block"
         onClick={handleClick}
         disabled={validPick}
   >
       <img src= {question.image } alt={question.alt}/>
       <h3>{question.text}</h3>
       <p>
          <a href ={question.image}>{question.credit} </a>
          <a href="https://unsplash.com">Unsplash</a>


       </p>
   </button>
  );
}

export default QuestionBlock

QuestionsBlock.tsx
import React, { forwardRef } from 'react'
import { Content,Question } from '../../interfaces'
import QuestionBlock from './QuestionBlock'

const QuestionsBlock = ({
  quizItemId,
  chosenAnswerItems,
  setChosenAnswerItems,
  unansweredQuestionIds,
  setUnansweredQuestionIds,
} : {
  quizItem: Content,
  chosenAnswerItems: string[],
  setChosenAnswerItems: function 
  unansweredQuestionIds: number [] | undefined
  setUnansweredQuestionIds: function
}, ref: React.LegacyRef<HTMLHeadingElement> | undefined ) => {
  return (
    <>
      <h2 ref={ref} className="title-block" }>{quizItem.text}</h2>
      <div className= "questions-container">
        {quizItem?.questions.map((question:Question, _index: number)=>
        <QuestionBlock/>
        key={{_index}}
        quizItemId={quizItem.id}
        question={question}
        chosenAnswerItems={chosenAnswerItems}
        setChosenAnswerItems {setChosenAnswerItems}
        setUnansweredQuestionIds={setUnansweredQuestionIds}
        ))} 
        
      </div>
    </>
  );
}

export default forwardRef (QuestionsBlock)


AnswerBlock.tsx
import React, {useEffect, useState, forwardRef} from 'react'
import {Answer} from '../../interfaces'

const AnswerBlock ({
  answerOptions, 
  chosenAnswers
} : {
  answerOptions:Answer[]  | undefined>,
  chosenAnswer: string[]
  
}, ref: HTMLDiveElement | any) => {
  
   const [result, setResult] = useState <Answer | null>()

  useEffect(() => {
    answerOptions?.forEach((answer:Answer) => 
    if (
      chosenAnswers.includes (answer.combination[0]) &&
      chosenAnswers.includes (answer.combination[1]) &&
      chosenAnswers.includes (answer.combination[2]) &&

    ) {
      setResult (answer)
    }

    })
    
  }, [chosenAnswer])

  console.log(result)

  return (
    <div ref={ref} className="answer-block">
      <h2>result?.text</h2>
      <img src= {result?.img} alt={result?.text} />
    </div>
  );
}

export default forwardRef (AnswerBlock)


Index.tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import './index.css'
import App from './App'


const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
)
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)


Interfaces.tsx
interface QuizData{
    title: string;
    subtitle: string;
    quizId: string;
    content: Content[];
    answers: Answer;

}

interface Answer {
    text: string;
    image: string;
    alt: string;
    combination: string[]
    
}

interface Content {
    id: number;
    text: string;
    questions: Question[];
}

interface Question {
    text: string;
    image: string;
    alt: string;
    credits: string;

}

export {QuizData, Answer, Content, Question}


Server.Ts
import express, {Request, Response} from 'express'
import axios, {AxiosRequest, AxiosResponse} from 'axios'
import { QuizData } from './interfaces'
import * as dotenv from 'dotenv'
dotenv.config() 

const PORT = 8000
const app = express()


app.get ('/ quiz-item', async (req: Request, res: Response) => {
    try {
        //@ts-ignore
        await const response: AxiosResponse = await axios.get (process.env.URL), {
        axios.get("url") {
            Headers: {
                'X-Cassandra-Token': process.env.TOKEN,
                accept: 'application/json'

            }
        })
        if (response.status === 200){
            const quizItem: QuizData = await response.data.['...']
            res.setHeader(name: 'Access-Control-Allow-Origin', 'https://localhost:3000')
            res.send(quizItem)
        }
    } catch (err) {
        console.error(err)
    }
})

app.listen(PORT, callback: () => console.log ('sever is running on port' + PORT))


Sample-Data.Json
{
    "quizId" : "3483j33",
    "text" : "what cheese are you ?",
    "subtitle" : "This quiz is not chessy or anything like that ..."
    "content" : [
    {
        "id" : 0,
        "text" : "Pick a vacation destination",
        "questions" : [
            {
                "text" : "New York", 
                "image" : "https://cdn.pixabay.com/photo/2017/02/11/11/27/nyc-2057534__480.jpg",
                "alt" : "Photo os the Central Park during daytime",
                "credit" : "Oliver Niblett" 
            },
        ]   
      },
      {
          "id" : 1,
          "text" : "Pick a home : ",
          "questions" : [
              {
                "text" : "Austin", 
                "image" : "https://a.travel-assets.com/findyours-php/viewfinder/images/res70/24000/24227-Lady-Bird-Lake.jpg",
                "alt" : "Photo os the Central Park during daytime",
                "credit" : "Burgess Milner"
              }
          ]
      }
    ],
    "answers" : [
        {
            "combination" : ["New York", "Pizza", "Traditional"],
            "text" : "Blue Cheese",
            "image" : "https://img.itdg.com.br/tdg/images/blog/uploads/2022/07/5-itens-necessarios-para-se-tornar-um-pizzaiolo-neste-Dia-da-Pizza.jpg?mode=crop&width={:width=%3E150,%20:height=%3E130}",
            "alt" : "blue cheese"
        },
    ]
  }
  
  Package.json
  {
  "name": "my-app",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^13.5.0",
    "@types/jest": "^27.5.2",
    "@types/node": "^16.18.2",
    "@types/react": "^18.0.24",
    "@types/react-dom": "^18.0.8",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1",
    "typescript": "^4.8.4",
    "web-vitals": "^2.1.4"
  },
  "scripts": {
    "start:frontend":"react-scripts start",
    "start:backend": "nodemon server.ts",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}



Title.Tsx
import React from 'react'
import Title from './Components/Title'


const Title() = ({title, subtitle}) : {
    title: QuizData['title'] | undefined,
    subtitle: QuizData['subtitle'] | undefined,
}) => {


  return (
    <div>

      <h1>{title}</h1>
      <p>{subtitle}</p>
      
    </div>
  );
}

export default Title



.env
TOKEN= 'X-Cassandra-Token': ...
URL=  (...)



  











New Terminal 
npx create-react-app my-app –template typescript 

DataStax Database 
npm i express axios nodemon

DataStax Database 
Connect 
Document API 

X-Cassandra Token 
Namespace ID
Body 

Launching Swagger UI
Post ( Quizes ) 
{
“name”: Quirky_Quizes 
}
Execute 

Post ( Quizes ) 
Quirky_Quizes 
{
}


Body
The JSON document 
Edite Value Model
{
    "quizId" : "3483j33",
    "text" : "what cheese are you ?",
    "subtitle" : "This quiz is not chessy or anything like that ..."
    "content" : [
    {
        "id" : 0,
        "text" : "Pick a vacation destination",
        "questions" : [
            {
                "text" : "New York", 
                "image" : "https://cdn.pixabay.com/photo/2017/02/11/11/27/nyc-2057534__480.jpg",
                "alt" : "Photo os the Central Park during daytime",
                "credit" : "Oliver Niblett" 
            },
        ]   
      },
      {
          "id" : 1,
          "text" : "Pick a home : ",
          "questions" : [
              {
                "text" : "Austin", 
                "image" : "https://a.travel-assets.com/findyours-php/viewfinder/images/res70/24000/24227-Lady-Bird-Lake.jpg",
                "alt" : "Photo os the Central Park during daytime",
                "credit" : "Burgess Milner"
              }
          ]
      }
    ],
    "answers" : [
        {
            "combination" : ["New York", "Pizza", "Traditional"],
            "text" : "Blue Cheese",
            "image" : "https://img.itdg.com.br/tdg/images/blog/uploads/2022/07/5-itens-necessarios-para-se-tornar-um-pizzaiolo-neste-Dia-da-Pizza.jpg?mode=crop&width={:width=%3E150,%20:height=%3E130}",
            "alt" : "blue cheese"
        },
    ]
  }
Execute 
Response body 

npm run start:backend
npm i ts-node
npm i @types/express @types/node





Get 
Namespace id - quizes
Collection id - quirky_quizes
Execute 

Request Url 

https://localhost:8000/quiz-item 

npm .dotenv
npr run start: frontend

npr run start: frontend
