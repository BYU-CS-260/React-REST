# React-REST

You can also access REST services from React.  This example will show you how easy it is to mix fetch calls with React to display the data.  In this tutorial, we will show you how to access the Utah cities REST service from your React application.

1. First create your index.html file with a form where the user can type in the first letters of a city:
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  </head>
  <body>
    <div id="root"></div>
    <script type="text/babel">  
    class CityForm extends React.Component {
      constructor(props) {
        super(props);
        this.state = {value: ''};
    
        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
      }
    
      handleChange(event) {
        alert('A name was changed: ' + event.target.value);
        this.setState({value: event.target.value});
      }
    
      handleSubmit(event) {
        alert('A name was submitted: ' + this.state.value);
        event.preventDefault();
      }
    
      render() {
        return (
            <form onSubmit={this.handleSubmit} onKeyUp={this.handleChange}>
            <label>
              Name:
              <input type="text" value={this.state.value} onChange={this.handleChange} />
            </label>
            <input type="submit" value="Submit" />
            </form>
        );
        }
    }
    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<CityForm />);
    </script>
  </body>
</html>
```
Notice that we are going to capture the keyup event and will call 'handleChange' when this occurs.  The state variable 'value' will contain the characters that have been typed into the box.  

2. Now add an array of cities to your state and add code to create an unordered list of cities once you have fetched them. 

```
var app = new Vue({
  el: '#app',
  data: {
    cities: [],
    prefix: '',
  },
});
```
Notice that we define the 'cities' array and the 'prefix' model variable.

3. Add the 'fetchREST' function and make sure that it is called with the correct 'prefix'.
```
  methods: {
    fetchREST() {
      console.log("In Fetch " + this.prefix);
    },
  },
```
4. Now add the code to fetch the cities that start with 'prefix' from the REST service. Once the data has been returned, the 'then' promise calls the 'json()' promise to convert the json to a javascript array.  Since this is an asynchronous call, we add another then when the conversion completes.  At this point, the 'cities' array is truncated and the cities from the REST service are pushed onto the 'cities' array.  They will then automatically be displayed with the mustache syntax in the html file.
```
      var url = "http://bioresearch.byu.edu/cs260/jquery/getcity.cgi?q=" + this.prefix;
      console.log("URL " + url);
      fetch(url)
        .then((data) => {
          return (data.json());
        })
        .then((citylist) => {
          console.log("CityList");
          console.log(citylist);
          this.cities = [];
          for (let i = 0; i < citylist.length; i++) {
            console.log(citylist[i].city);
            this.cities.push({ name: citylist[i].city });
          };
          console.log("Got Citylist");
        });
```
