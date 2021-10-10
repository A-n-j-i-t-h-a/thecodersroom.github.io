
#include <iostream>
using namespace std;

template <class T>
class LinkedList
{
    class Node
    {
        friend class LinkedList;
        T data;
        Node *next;
        public:
            Node(T data, Node *next = NULL)
            {
                this->data = data;
                this->next = next;
            }
    } *head, *tail;
    int _size;
    
    public:
        LinkedList();
        ~LinkedList();
        bool isEmpty();
        int size();
        void addAtHead(T);
        void addAtTail(T);
        T removeFromHead();
        T removeFromTail();
        void insert(T, int);
        T remove(int);
        int find(T);
        T findKth(int);
        void print();
};

template <class T>
LinkedList<T>::LinkedList()
{
    head = tail = NULL;
    _size = 0;
}

template <class T>
LinkedList<T>::~LinkedList()
{
    Node *currentNode = head;
    while(currentNode)
    {
        Node *next = currentNode->next;
        delete currentNode;
        currentNode = next;
    }
    head = NULL;
}

template <class T>
bool LinkedList<T>::isEmpty()
{
    return _size == 0;
}

template <class T>
int LinkedList<T>::size()
{
    return _size;
}

template <class T>
void LinkedList<T>::addAtHead(T data)
{
    head = new Node(data, head);
    if(!tail)
        tail = head;
    _size++;
}

template <class T>
void LinkedList<T>::addAtTail(T data)
{
    if(!head)
        addAtHead(data);
    else
    {
        Node *newNode = new Node(data);
        tail->next = newNode;
        tail = tail->next;
        _size++;
    }
}

template <class T>
T LinkedList<T>::removeFromHead()
{
    Node *temp = head;
    T retValue = head->data;
    head = head->next;
    delete temp;
    _size--;
    if(!head)
        tail = NULL;
    return retValue;
}

template <class T>
T LinkedList<T>::removeFromTail()
{
    T retValue = tail->data;
    if(head == tail)
    {
        head = tail = NULL;
        _size--;
        return retValue;
    }
    Node *secondLastNode = head;
    while(secondLastNode->next->next)
        secondLastNode = secondLastNode->next;
    delete secondLastNode;
    _size--;
    return retValue;
}

template <class T>
void LinkedList<T>::insert(T data, int position)
{
    if(position > _size + 1)
        cout << "\nNot a valid position!";
        
    else if(position == 1)
        addAtHead(data);
    else if(position == _size + 1)
        addAtTail(data);
    else
    {
        Node *currentNode = head;
        for(int i = 1; i < position - 1; i++)
            currentNode = currentNode->next;
        currentNode->next = new Node(data, currentNode->next);
        _size++;
    }
}

template <class T>
T LinkedList<T>::remove(int position)
{
    if(position == 1)
        return removeFromHead();
    
    if(position == _size)
        return removeFromTail();
        
    Node *currentNode = head;
    for(int i = 1; i < position - 1; i++)
        currentNode = currentNode->next;
    T retValue = currentNode->next->data;
    currentNode->next = currentNode->next->next;
    _size--;
    return retValue;
}

template <class T>
int LinkedList<T>::find(T item)
{
    Node *currentNode = head;
    for(int i = 1; currentNode; i++)
    {
        if(currentNode->data == item)
            return i;
        currentNode = currentNode->next;
    }
    return -1;
}

template <class T>
T LinkedList<T>::findKth(int position)
{
    Node *currentNode = head;
    for(int i = 1; i != position; i++)
        currentNode = currentNode->next;
    return currentNode->data;
}

template <class T>
void LinkedList<T>::print()
{
    if(isEmpty())
    {
        cout << "\nLinked List is empty!";
        return;
    }
    Node *currentNode = head;
    while(currentNode)
    {
        cout << currentNode->data << " ";
        currentNode = currentNode->next;
    }
}

template <class T>
class QueueLinked
{
    LinkedList<T> queue;
    public:
        inline bool isEmpty() {
            return queue.isEmpty();
        }
        void enqueue(T);
        T dequeue();
        void print();
};

template <class T>
void QueueLinked<T>::enqueue(T item)
{
    queue.addAtTail(item);
}

template <class T>
T QueueLinked<T>::dequeue()
{
    return queue.removeFromHead();
}

template <class T>
void QueueLinked<T>::print()
{
    queue.print();
}

int main()
{
    QueueLinked<int> q;
    int ele, choice = 0;
    while(choice != 4)
    {
        // cout << "\n1. Insert\n2. Delete\n3. List\n4. Exit"
        //      << "\nEnter the choice ";
        cin >> choice;
        switch(choice)
        {
            case 1:
                // cout << "\nEnter the element to insert ";
                cin >> ele;
                q.enqueue(ele);
                break;
            
            case 2:
                if(q.isEmpty())
                    cout << "\nQueue is empty!";
                else
                {
                    cout << endl << q.dequeue() << endl;
                    // cout << "\nDeleted element is " << q.dequeue();
                }
                break;
            
            case 3:
            // cout << "\nElements in the queue ";
                q.print();
                break;
        }
    }
}
