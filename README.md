  # Code Demonstration for Hash Tables
This repo contains the code and relevant details for the code demonstration (Assignment 6) for the course MSDS-610, part of the MS in Data Science program at the University of San Francisco.  
This repo contains contributions by the following team members (mail ids are mentioned in hyperlink):
- [Devendra Govil](mailto:dgovil@dons.usfca.edu)
- [Kayhan Eryilmaz](mailto:kkeryilmaz@dons.usfca.edu)
- [Ity Soni](mailto:isoni@dons.usfca.edu)

# Agenda
- Implementation of Linear Search and Hash Tables
- Compare Run times for both
- Look at trends in run times for different bucket sizes vis-a-vis linear search

# Source of Data
The data set for books has been taken from the following Kaggle page:  
https://www.kaggle.com/datasets/saurabhbagchi/books-dataset?resource=download

# Recap of Previous Discussion
## What are Hash Tables?
Hashtables are useful data structures that allow us to quickly organize, sort, and look up data.  
The 4 important components for generating and using hashtables are: 
- keys
- values
- bins
- hash function
    - This is the central component that is used to assign keys to respective bins.
## Why do we need Hash Tables?

In data science, we always come across large data sets and parsing through the data can be a cubersome task when we're searching for something we don't know the location of. The most simplistic way to search for a value in a list of values would be to iterate through it and stop once we find it. With hashtables we can speed up the process and make our lives much easier with constant look up times.

# Linear Search
## Linear Search Implementation
```
def find_in_list(object_list, book_name):
    """
    Find the book_name entry in the object list tuples.
    Parameters:
        object_list: list[tup[str,str]]
        book_name: str
    Returns:
        Return value is the
        ('Book-Title','Book-Author') tuple from the object_list
    """
    book_name = book_name.lower()
    for entry in object_list:
        if book_name == entry[0]:
            return entry
    return (0, 0)
```  
This function goes through the list of tuples one by one and looks for the object. As you can imagine, the lookup time scales linearly with data set size as can be seen in the image below.  
> The Graphs in this repo are clearly visible only in light mode. Graphs aren't compatible with Dark mode currently.  
![Lookup times for linear search](https://github.com/devendragovil/USFMSDS_610_Assignment6/blob/main/resources/linear_search_output.png)

# Hash Table Implementation

We implement a hash table as a list of lists. The bucket value defines the first index of this 2-D array that will be used to store our key-value pair.  
For this, we first implement a function to generate an empty hash table as shown below.  
```
def empty_hashtable(buckets):
    """
    Function takes number of buckets and returns a list of buckets size empty list
    Parameters:
        buckets: int
    Returns:
        Return value is list of empty lists.
        ReturnValue: list[list[]]
    """
    lol = []
    for i in range(buckets):
        lol.append([])
    return lol
```

We then define our hash function to generate the hash code value for a given key, as shown below.  
```
def hash_function(key):
    """
    Function takes a key value and returns a hash code value for that key.
    Parameters:
        key: str
    Returns:
        ReturnValue: int
        Return value is the hash code of the function
    """
    h = 0
    key = key.lower()
    for a in key:
        h = ((h * ord(a))) % 2147483647 + ord(a)
        # Dividing to stop the hash code from going excessively
        # and arbritrary large
        # Dividing by a prime to evenly space the hash code distributions
        # Interesting fact: 2147483647 is the 8th Mersenne Prime.
        # hash code is returned
    return h
```

After this we define a function to populate the hash table. This might take some time, however the investment in generating the hashtable is easily recouped when we are doing repeated lookups which is the case in real life.  

```
def populate_hashtable(list_of_tups, buckets):
    """
    Function takes a list of tuples (of two strings, books and authors),
    and buckets to populate a hashtable
    Parameters:
        list_of_tups: list[tup[str,str]]
        buckets: int
    ReturnValue:
        required_ht: list[list[str,str]]
    ReturnValue is the populated hashtable of list of lists
    """
    required_ht = empty_hashtable(buckets)
    for entry in list_of_tups:
        book = entry[0]
        author = entry[1]
        hashcode_book = hash_function(book)
        bucket_book = hashcode_book % buckets
        required_ht[bucket_book].append(entry)
    return required_ht
```

We then finally implement our search function. This takes our hashtable and looks for the specific key that we specify. 

```
def find_in_hashtable(given_ht, value_to_find):
    """
    Function takes the hashtable and value_to_find as paramter inputs
    to look for the value_to_find in the hash
    table, return the tuple if it finds it, else return (-1,-1)
    Parameters:
        given_ht: list[list[str,str]]
        value_to_find: str
    ReturnValue:
        entry: (str,str)
            : (-1,-1) if entry in hashtable is not found
    """
    hashcode_tofind = hash_function(value_to_find)
    buckets_ht = len(given_ht)
    bucket_tofind = hashcode_tofind % buckets_ht
    for entry in given_ht[bucket_tofind]:
        if entry[0] == value_to_find:
            return entry
    return (-1, -1)
```

## Impact of number of buckets/bins on search times

As we increase the number of buckets, we decrease the search space by that factor (assuming an approximately uniform assignment of keys to buckets). This means that the search time decreases with increasing buckets. In the extreme case, when we have a __perfect hash function__ which maps every key to a unique bucket, we reach the desired scenario of lookup times that are independent of data size.  
  
We have generated lookup times for various buckets and compare them with each other as well as with linear search. We can see (from the image below) that lookup times decrease with increasing buckets.  
> The Graphs in this repo are clearly visible only in light mode. Graphs aren't compatible with Dark mode currently. 
![Lookup times for hashtables and linear search](https://github.com/devendragovil/USFMSDS_610_Assignment6/blob/main/resources/hashtable_linear_comparison.png)

# Conclusion
We can draw the following conclusions:
- Hash Tables can be implemented as a list of list
- Hash Tables (specially with sufficient buckets) have very good performance vis-a-vis linear search
- Lookup Times decrease when we increase buckets and are constant in the case of the perfect hash function
- Hash Tables are indispensable for large data sets as they provide a major speed up over linear search 
- The initial time to build a hashtable is easily compensated when performing multiple lookups