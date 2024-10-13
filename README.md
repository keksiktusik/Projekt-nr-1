#include <iostream>

class DoublyLinkedList {
private:
    struct Node {
        int data;
        Node* next;
        Node* prev;
        Node(int value) : data(value), next(nullptr), prev(nullptr) {}
    };

    Node* head;
    Node* tail;

public:
    DoublyLinkedList() : head(nullptr), tail(nullptr) {}

    // dodanie elementu na początek
    void addToStart(int value) {
        Node* newNode = new Node(value);
        if (!head) {
            head = tail = newNode;
        } else {
            newNode->next = head;
            head->prev = newNode;
            head = newNode;
        }
    }

    // dodanie elementu na końcu
    void addToEnd(int value) {
        Node* newNode = new Node(value);
        if (!tail) {
            head = tail = newNode;
        } else {
            tail->next = newNode;
            newNode->prev = tail;
            tail = newNode;
        }
    }

    // Dodanie określonego indexu
    void addAtIndex(int index, int value) {
        if (index == 0) {
            addToStart(value);
            return;
        }
        Node* newNode = new Node(value);
        Node* temp = head;
        for (int i = 0; i < index - 1 && temp; i++) {
            temp = temp->next;
        }
        if (!temp) {
            std::cout << "Indeks poza zakresem.\n";
            delete newNode;
            return;
        }
        newNode->next = temp->next;
        newNode->prev = temp;
        if (temp->next) {
            temp->next->prev = newNode;
        } else {
            tail = newNode;
        }
        temp->next = newNode;
    }

    // Usunięcie elementu z początku
    void removeFromStart() {
        if (!head) {
            std::cout << "Lista jest pusta.\n";
            return;
        }
        Node* temp = head;
        head = head->next;
        if (head) {
            head->prev = nullptr;
        } else {
            tail = nullptr;
        }
        delete temp;
    }

    // Usunięcie elementu z konca
    void removeFromEnd() {
        if (!tail) {
            std::cout << "Lista jest pusta.\n";
            return;
        }
        Node* temp = tail;
        tail = tail->prev;
        if (tail) {
            tail->next = nullptr;
        } else {
            head = nullptr;
        }
        delete temp;
    }

    // usunięcie określonego elementu
    void removeAtIndex(int index) {
        if (index == 0) {
            removeFromStart();
            return;
        }
        Node* temp = head;
        for (int i = 0; i < index && temp; i++) {
            temp = temp->next;
        }
        if (!temp) {
            std::cout << "Index poza zakresem.\n";
            return;
        }
        if (temp->prev) {
            temp->prev->next = temp->next;
        }
        if (temp->next) {
            temp->next->prev = temp->prev;
        } else {
            tail = temp->prev;
        }
        delete temp;
    }
    
    // wyświetlenie listy
    void displayList() const {
        Node* temp = head;
        while (temp) {
            std::cout << temp->data << " ";
            temp = temp->next;
        }
        std::cout << std::endl;
    }

    // wyświetlenie listy w odwrotnej kolejności
    void displayReverse() const {
        Node* temp = tail;
        while (temp) {
            std::cout << temp->data << " ";
            temp = temp->prev;
        }
        std::cout << std::endl;
    }

    // wyczyszczenie listy
    void clearList() {
        while (head) {
            removeFromStart();
        }
    }

    // destruktor do czyszczenia pamięci
    ~DoublyLinkedList() {
        clearList();
    }
};

// Test wszystkich funckji w mainie:
int main() {
    DoublyLinkedList list;

    list.addToStart(5);
    list.addToStart(10);
    list.addToEnd(20);
    list.addAtIndex(1, 15);

    std::cout << "List: ";
    list.displayList();

    std::cout << "Lista w odwrotnej kolejności ";
    list.displayReverse();

    list.removeFromStart();
    std::cout << "{Po usunięciu z początku}: ";
    list.displayList();

    list.removeFromEnd();
    std::cout << "Po usunięciu z końca: ";
    list.displayList();

    list.removeAtIndex(1);
    std::cout << "Po usunięciu z indeksu 1: ";
    list.displayList();

    list.clearList();
    std::cout << "Po wyczyszczeniu listy: ";
    list.displayList();

    return 0;
}
