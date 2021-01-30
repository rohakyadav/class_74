Topic Lazy Loading

● Display transactions in a Flatlist. 

● Build a search bar to query transactions collection to search for a field value. 

● Display the query results in a Flatlist.

What should our user be able to do with the search screen?

User should be able to search for a book id or student id and the screen should display the last transactions for the book id and student id

There might be a huge number of transactions for any student id or book id. How would we display them on the screen?

Using ScrollView. The user could scroll through the list of all the transactions.

That is one way to do it. However, ScrollView takes and renders all the items in the list at once. It takes time for the javascript engine to load and render all the data. This will make the app less performant.

There is another kind of React Component which lazily loads the list.

It loads only a list of items which can be displayed on one screen. When the user scrolls down, it fetches new data, adds them to the list and renders it on the screen. This react component is called a FlatList

Facebook, Twitter and other kinds of apps where there is a newsfeed.

let us display ALL the transaction documents from transaction collections in our search screen.

\1. let us display ALL the transaction documents from transaction collections in our search screen.

We will create a state array which will hold all the transactions.

When the screen mounts, we will query all the transactions and store them in the state array.

In the render function for the component, we will map over the state array and display each item.

\2. Let's store the entire list of transactions inside this state when the component mounts.\2. Let's store the entire list of transactions inside this state when the component mounts.

Let's render a ScrollView in our render function. Inside ScrollView, we are going to write some javascript code. We will map over the allTransactions state array. For each element in the array, we will return a View component containing information about that transaction using Text Components. *Note: Use curly brackets before writing Javascript code. toDate() function converts the firebase data value (in ms) to date string.

```jsx
   <ScrollView>
{this.state.allTransactions.map((transaction,index)=>{
<View  key={index}style = {{borderBottomWidth:2}}> 
      <Text>
         {"Book Id"+" "+transaction.bookId}
         </Text>
         <Text>
         {"student Id"+" "+transaction.studentId} 
         </Text>
         <Text>
         {"transaction Type"+" "+ transaction.transactionType} 
         </Text>
         <Text>
         {"date"+ " "+transaction.date.toDate()} 
         </Text>
 </View> ) })}
        </ScrollView>)}}
```

You might get a warning for giving a unique key prop for all the items in the list. A unique prop helps each list item to be identified and that helps in rendering the items. It is good practice to add a unique key to each component in the list. We will use the array index as the unique key here.

t it takes a while for the items to be rendered inside scrollView

This is because, before rendering it waits to get all the items and only then renders them.

This will make our app less performant. To avoid that, we use FlatList.

FlatList has 3 key props: ● data: This contains all the data in the array which needs to be rendered. ● renderItem: This takes each item from the data array and

renders it as described using JSX. This should return a JSX component. ● keyExtractor: It gives a unique key prop to each item in the list. The unique key prop should be a string.

in FlatLists, we are waiting for all the transaction documents in ComponentDidMount before rendering these items.

We can get only the first 10 transactions in componentDidMount. This will be quicker than getting all the transactions from the collection. Let's store the last transaction doc which we get inside another state called lastVisibleTransaction.

FlatList has two more important props. onEndReached and onEndThreshold ● onEndReached can call a function to get more transaction documents after the last transaction document we fetched. ● onEndThreshold defines when we want to call the function inside onEndReached prop. If onEndThreshold is 1, the function will be called when the user has completely scrolled through the list. If onEndThreshold is 0.5, the function will be called when the user is mid-way during scrolling the items.

Let's put the onEndReachedThreshold as 0.7 and write a function to fetch more

transaction documents after the last transaction document we fetched.

We will have to create a search TextInput which takes book id or student id. After clicking the search button, a function should be called which takes the input entered by the user and queries the database. All the results of the query are added to the state array storing the list of items to be displayed.

design the Search Bar. It will contain a TextInput and

When search button is pressed, let's call a function searchTransactions() searchTransactions will first check if the entered text is a book id or a student id (begins with B or S) Depending on that, the function will query the transactions collection matching the book id or student id and add them to the state allTransactions array. Limit the fetched transactions to 10.

We will have to modify fetchMoreTransactions to fetch more transactions that match the student or Book id.