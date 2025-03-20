## action属性とuseActionStateを使った場合のコード例

```js
// Form.jsx

const Form = (props) => {
    return (
        <form action={props.getWeather}>                
            <input type="text" 
                   name="city" 
                   placeholder="都市名"
            />          
            <button type="submit">
                Get Weather
            </button>
        </form> 
    )
}

export default Form
```

```js
// App.jsx

import { useActionState } from "react" 
import Title from "./components/Title"
import Form from "./components/Form" 
import Results from "./components/Results"
import Loading from "./components/Loading" 

const App = () => {
    const getWeather = (prevState, formData) => {
        const city = formData.get("city")
        return fetch(`https://api.weatherapi.com/v1/current.json?key=xxxyyyzzz&q=${city}&aqi=no`) 
                .then(res => res.json())  
                .then(data => data)        // このラインは省略可
    }

    const [returnedData, formAction, isPending] = useActionState(getWeather, "")

    return (
        <div className="wrapper">
            <div className="container">
                <Title/> 
                <Form getWeather={formAction}/>
                {isPending ? <Loading/> : <Results results={returnedData}/>}
            </div>
        </div>
    )
}

export default App
```


```js
// Results.jsx

const Results = (props) => {
    return (
        <>          
            {props.results.location?.country && 
                <>
                    <div className="results-country">{props.results.location.country}</div>
                    <div className="results-city">{props.results.location.name}</div>
                    <div className="results-temp">{props.results.current.temp_c} <span>°C</span></div>
                    <div className="results-condition">
                        <img src={props.results.current.condition.icon} alt="icon"/>
                        <span>{props.results.current.condition.text}</span>
                    </div>
                </>
            }
        </>
    )
}

export default Results
```
