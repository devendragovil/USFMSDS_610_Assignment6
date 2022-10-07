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
![Lookup times for linear search](https://www.git)