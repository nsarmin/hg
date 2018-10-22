# hg###############################################################
# This is a node-based implementation code of a linked list (Problem 2A). In this portion,
# I define several classes and functions such as bubble sort, merge sort, insertion, swaping.
# Author: Nusrat Sarmin
# Date: 10/18/2018
##############################################################

class Node(object):
    def __init__(self, item, next=None):
        self.item = item #instance of the object itself.
        self.next = next

# initialzing head
class LinkedList:
	def __init__(self):
		self.head = None

#Insert a new node containing item at position head.
	def insert(self, newNode):
		if self.head is None:
			self.head = newNode # create new node if head is empty
		else:
			lastNode = self.head
			while True:
				if lastNode.next is None:
					break
				lastNode = lastNode.next
			lastNode.next = newNode
			
#Printing linked list			
	def ShowList(self):
		if self.head is None:
			print('list is Empty /n')
			return
		curNode = self.head
		while True:
			if curNode is None:
				break
			if curNode.next is None:
				print(curNode.item)
				return
			subNode = curNode.next
			if curNode.item == subNode.item:
				print('ID matches:', curNode.item)
			else:
				print(curNode.item)
			curNode = curNode.next
			        
# Duplicate Id.
	def ShowDuplicate(self, count):
		if self.head is None:
			print('list is Empty /n')
			return
		curNode = self.head
		while True:
			if curNode is None:
				break
			if curNode.next is None:
				return print(curNode.item)
			subNode = curNode.next
			if count[curNode.item] == True:
				print("Matching ID: ", curNode.item)
			else:
				print(curNode.item)
			curNode = curNode.next

# swapping items      
def swapped(a,b):
	temp = a.item
	a.item = b.item
	b.item =temp
   
#Bubble sort technique
def bubblesort(head):
	swap=True
	while swap:
		swap =False
		ptr=head
		currentNode=head.next
		while currentNode!=None:
			if (ptr.item < currentNode.item):
				swapped(ptr,currentNode)
				swap = True
			currentNode = currentNode.next
			ptr=ptr.next

# dividing a linked list into two equal linked lists
def divideLists(item):
    half1 = item
    half2 = item
    if half2:
        half2 = half2.next            
    while half2:
        half2 = half2.next   
        if half2:
            half2 = half2.next
            half1 = half1.next
    mid = half1.next
    half1.next = None
    return item, mid

# Merging two half linked lists from mergesort 
def mergeLists(part1, part2):
    if part1== None and part2==None:
        return None
    if part1==None:
        return part2
    if part2==None:
        return part1
    if part1.item > part2.item:
        tmp = part1
        part1=part1.next
    else:
        tmp=part2
        part2=part2.next
    lastNode=tmp
    while part1!=None and part2!= None:
        if part1.item>part2.item:
            lastNode.next=part1
            part1 = part1.next
        else:
            lastNode.next=part2
            part2=part2.next
        lastNode=lastNode.next
    if part1!=None:
        lastNode.next=part1
    elif part2!=None:
        lastNode.next=part2
    return tmp
   
# Merging sort (recursive)technique
def mergeSort(idlist):
	if idlist is None or idlist.next is None:
		return idlist
	lefthalf, righthalf = divideLists(idlist)
	lefthalf = mergeSort(lefthalf)
	righthalf = mergeSort(righthalf)
	return mergeLists(lefthalf, righthalf)
	
# Define a boolean array.
def Last(n):
	seen=[False]*6001 #m+1. Here, the value of m=6000
	count =[0]*6001
	while n is not None:
		count[n.item]=count[n.item]+1
		if count[n.item]>1:
			seen[n.item] = True
		n=n.next
	return seen


# function to read files.
def read_id(fileName):
	file = open(fileName, "r") #read
	for line in file:
		x = int(line.strip()) #converting string to integer
		node = Node(x)
		mainlist.insert(node)
	
# define a single liked list
mainlist = LinkedList()

# Read two files from problem
read_id("activision.txt")
read_id("vivendi.txt")

# Prinitng the single liked lsit which stores all Ids; Comparing to find duplicates
print('Here is the Employee IDs in a single list: \n', mainlist.ShowList())

print('Comparing to find duplicate IDs:')
if mainlist.head is None:
	print("list is Empty")
curNode = mainlist.head
while True:
	if curNode is None:
		break
	if curNode.next is None:
		print("None")
	subNode = curNode.next
	while True:
		if subNode is None:
			break
		if curNode.item == subNode.item:
			print("Duplicate id is:", curNode.item)
		subNode = subNode.next
	curNode = curNode.next

# Sorting list using Bubble sort and finding duplicates
print('Bubble sort: \n', bubblesort(mainlist.head))
print('Duplicates:\n', mainlist.ShowList())

# Sorting list using Merge sort and finding duplicates
print('Merge sort \n', mergeSort(mainlist.head))
print( 'Duplicates:\n', mainlist.ShowList())

# boolean array(seen) of length-6001 to indicate if elements have been seen before.
barray = Last(mainlist.head)
print('The duplicates through a single pass into unsorted list is:\n',mainlist.ShowDuplicate(barray))
