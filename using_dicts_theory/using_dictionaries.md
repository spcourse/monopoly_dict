
# Common dictionary patterns

You’ve read a lot about the efficiency of dictionaries, but next we’ll focus more on the common usage patterns for dictionaries

## 0. Fast Search

As efficiency is such an important part of the dictionaries, the most common usage pattern is still doing fast searches. As a very simple example, lets say you’ve got the following dictionary defined containing names of students and their student numbers

    student_ids = {‘Jack’: 1234,  ‘Jim’: 1237, ‘John’: 1258, ‘James’: 1241}

Now you want to find the student number for ‘John’, no matter how large that list of students, what you can just write is

    student_name = ‘John’
    student_number = student_ids[student_name]

This means you don’t need to write a loop, which is a very common mistake for beginning programmers. You should **never** write a dictionary search like this

    student_name = ‘John’
    student_number = None

    for name in student_ids:
        if name == student_name:
            student_number = student_ids[name]

This version using a for loop will definitely be a lot slower, but you might not notice the difference in this until the data sets you’re working become very large. What should be obvious looking at the examples is that the version of the code without the loop is also faster to write and easier to read. This means there are fewer opportunities for mistakes and fixing any mistakes will also a simpler. Using dictionaries for fast search is not just about the efficiency, but also about being easier to write, read, and change if needed.

## 1. Mapping

The basic pattern for using a dictionary is a mapping, so building a data representation that links one piece of information to another. Any time you want to link two types of information together in code, you should think about using a dictionary.

Let’s consider the example where you’re reading a data file containing a list of dates and a list of the corresponding temperatures on these dates. This is essentially a mapping between dates and temperatures, so could also be represented using a dictionary

    date_list = [‘1/1/1980’, ‘2/1/1980’, ‘3/1/1980’, ‘4/1/1980’]
    temp_list = [12, 14, 13, 12]

    date_temp = {}

    for i in range(len(date_list)):
        date = date_list[i]
        temp = temp_list[i]
        date_temp[date] = temp

Building this dictionary still requires a for loop, but we’ll only need to do this once. Now if we want to search for the temperature at a specific date, we can just write something like 

    temp_jan_2nd = date_temp[‘2/1/1980’]

Note that dictionaries are **not ordered**, so if you want to go through all the temperatures in order (like you did the assignment), you should still use lists. However, if you want to search for temperatures on specific dates or find the student number of a specific student, dictionaries are the way to go.

## 2. Config/attributes

Another common pattern for dictionaries is to store some attributes or a configuration. This is still a type of mapping, but it is used to store some attributes that are *internal* in your code together in single representation.

An example here is easiest. Let’s say you want to make several different things configurable about a monopoly simulation you’re running. You could make them all arguments of the function, but especially if you have many different things to configure together, it might be more convenient to store them together in a dictionary

    monopoly_config = {'players': 2, 'board_size': 40}
    run_simulation(monopoly_config)

These types of mappings do not come from some data file like the temperatures, but store some attributes defined within your code together, where the **key** is always the name of the attribute. 

These dictionaries are usually defined in a single step like above, so don’t require any loops to create. Using the attributes inside the function is then just a simple search, like for example

    if position >= config[‘board_size’]:
        ... 

Note that because the keys are the names of the attributes, you’ll need to use string quote when searching for an attribute.

## 3. Counting

Another very common pattern for dictionaries is to count occurrences, for example when you want to count how often each word occurs in your data set. Without a dictionary, this would require two nested loops, which is rather messy and **very inefficient** in terms of big o complexity.

    word_list = [‘the’, ‘test’, ‘sentence’, ‘for’, ‘the’, ‘word’, ‘count’]

    for word_1 in word_list:
        count = 0
        for word_2 in word_list:
            if word_1 == word_2:
                count +=1
        
        print(f’{word_1} occurs {count} time in the list’).

A much better solution is to use a counting dictionary and a single loop

    word_list = [‘the’, ‘test’, ‘sentence’, ‘for’, ‘the’, ‘word’, ‘count’]
    word_count = {}

    for word in word_list:
        if word not in word_count:
            word_count[word] = 1
        else:
            word_count[word] += 1

    print(word_count)

Note that for this sort of dictionary pattern you’ll always need an if statement in your loop to distinguish two cases: One where you’ve not seen this particular key in your dictionary before and need to add the key to the dictionary with a count of 1, and other case where you have already seen the key before during the loop and need increment the count from before (so as to not overwrite it with the same 1 value again, but increase the count to 2, 3, 4 etc.)

## 4. Grouping

The final common pattern we’ll discuss here is when making a grouping, which is very similar to the previous pattern, but stores more information then just the amount of occurrences. Let’s take the same example from before, but now we want create a list of all the positions that each word occurs. This will require exactly the same kind of loop, but now if you haven’t seen this particular key in the dictionary before, you’ll need to a new list with just the current position to the dictionary. If you have seen the key before, then you’ll need to append the current position to the list of positions you’ve already added for that key.

    word_list = [‘the’, ‘test’, ‘sentence’, ‘for’, ‘the’, ‘word’, ‘count’]
    word_index = {}

    for i in range(len(word_list)):
        word = word_list[i]
        
        if word not in word_index:
            new_index_list = [i]
            word_index[word] = new_index_list
        else:
            existing_index_list = word_index[word]
            existing_index_list.append(i)

    print(word_index)

The final data representation is now is now a little more complex, as each value is now a whole list of positions, instead of just a count, but the structure of the code here is exactly the same.

Another common case when using groupings is when reversing a dictionary. Note that the keys in a dictionary must always be unique, as each key must always map to exactly one value, but values don’t need to be unique at all. So, if you’re trying to swap the keys and the values of dictionary, then you might have multiple identical values, each mapping to different key. The easiest way to solve this, is with a list values, like in the example above.

Let’s say you want to reverse the dictionary with dates and temperatures, so you want to construct a new dictionary where you can easily find all of the dates when a specific temperature was measured.

    date_temp = {‘1/1/1980’: 12, ‘2/1/1980’: 14, ‘3/1/1980’: 13, ‘4/1/1980’: 12}
    temp_date = {}

    for date in date_temp:
        temp = date_temp[date]
        
        if temp not in temp_date:
            temp_date[temp] = [date]
        else:
            temp_date[temp].append(date)

    print(temp_date)

Again, while the resulting representation is a little more complex because each value is also a list, the structure of the code remains exactly the same.


