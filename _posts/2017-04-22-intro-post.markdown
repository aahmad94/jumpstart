---
layout: post 
title: Hello World!
date: 2017-04-22 11:40:00
---

<p>App Academy reccomends blogging daily about what you're currently learning and working on. I think this will be a nice place to look back to and see the code from my previous work. Also the blog should motivate me to do more learning projects since it's a good way to track my progress and I want to keep adding things here!</p>

<p>I will be visiting App Academy on the 15th of May for the JumpStart program. It'll be my first time in California and I'm pretty excited about the visit. Feel free to contact me if you have any questions about my experience so far!</p> 

<p>Since this is my first post and I want to get some content here, I'll add the code to my first Python program, a to-do list!</p>

<p>Edit:</p>

<p>Yikes that took up more space than I thought it would... maybe I should work on making my code a little more succinct!</p>

<p>Also, another project that I worked on recently was the assessment assignment for Start Up Institute which entailed making using Ruby on Rails to make a small web app. It's supposed to send out texts using the Twilio API (pretty much just wrote 'gem install twilio' for that part). Unfortunately it only sends messages to Twilio verified numbers and I have yet to update it to make an exception for that so it'll crash if you don't enter a verified number. My number is verified though, so if you have it, feel free to send me a message from the web app!</p>

<p><a href="https://nameless-depths-56151.herokuapp.com">SMS-Heroku-webapp</a></p>

{% highlight python %}
# make a list to hold onto items
todo_list = []

# print out the list
def show_list():
    print("Here's your list:")

    for item in todo_list:
        print(item.upper())

# add new items to list
def add_to_list(new_item):
    todo_list.append(new_item)
    print(("Added {} to list; list now has {} items").format(new_item, len(todo_list)))

# print out instructions on how to use the app
def show_help():
    print("Enter 'HELP' to request instructions")
    print("Enter 'SHOW' to see what the list contains")
    print("Enter 'DELETE' to show list and delete a task")
    print("Enter 'DONE' to stop adding items and save as a .txt file.")
    print("""
Enter tasks to add to the list""")


def main():
    show_help()

    while True:
        # ask for new items
        new_item = input("> ")

        # be able to quit the app
        if new_item == "DONE":
            break

        # be able to show shopping list while entering items
        elif new_item == "SHOW":
            print('; '.join(todo_list).upper())
            continue

        # be  able to show instructions while entering items
        elif new_item == "HELP":
            show_help()
            continue

        # be able to delete an item from the list
        elif new_item == "DELETE":
            print(todo_list)
            try:
                item_value = int(input("What item would you like to delete (enter integer value for item index in list, item 1 = 1)? "))
                print("Item at index " + str(item_value) + " of list deleted.")
                del todo_list[item_value - 1]
                continue
            except ValueError:
                print("That's not an integer value, please enter the 'DELETE' command again." )
            except IndexError:
                print("There's no item with an index of that integer value, please enter the 'DELETE' command again")
            continue
        add_to_list(new_item)

    show_list()

main()

# be able to save list
with open("todo_list.txt", "w") as out_file:
    print("Here's your list: ", file = out_file)
    for item in todo_list:
        print("* " + item.upper(), file = out_file)
{% endhighlight %}

