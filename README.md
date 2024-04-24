#include <stdio.h>
#include <stdbool.h>
#include <time.h>
#include <stdlib.h>
#define size 10

//identifiers
typedef struct
{
    char name[20];
    int booked;
} Rooms;

enum StaffOptions {SeeRooms = 1, AddRoom = 2, RemoveRoom = 3}; 
enum GuestOptions {BookRoom = 1, ChangeRoom = 2, SeeBookings = 3};
enum Options {Staff, Guest};
enum Bookings {Open, Reserved, User};

//Functions
void printArray(Rooms list[]); // used for testing

int readInt(); // returns the user's integer input
void readChar(); // reads a string from the user
void generateBooked(Rooms list[]); // generates random booked rooms
void staff_menu(Rooms list[]); // staff menu
void guest_menu(Rooms list[]); // guest menu
void view_availability(); // view available Rooms
void add_room(Rooms list[]); // add Rooms
void remove_room(Rooms list[]); // remove Rooms
void book_room(Rooms list[]); // book Rooms
void change_rooms(Rooms list[]); // change Rooms
void view_bookings(Rooms list[]); // view bookings

int check_booking(Rooms list[], int room); // sees if the room is open, reserved, or booked by the user
int quantity(Rooms list[], int type); // sees how many rooms are open, reserved, or booked by the user

//main
int main()
{
    srand(time(NULL));
    Rooms list[size];
    generateBooked(list);
    printf("Are you a Staff (0) or Guest (1)? ");
    int ans = readInt();
    
    if (ans == Staff) // if the answer is 0, then take them to the staff menu
        staff_menu(list);
        
    else if (ans == Guest) // if the answer is 1, then take them to guest menu
        guest_menu(list);
        
    return 0;
}
//Function Statements

int readInt() // reads the user's input for user in correlation to array
{
    int ans;
    scanf("%d", &ans);
    return ans;
}

// *** Might need to delete this function if we cannot find a solution ***
void readChar(char name[])//read name input
{
    char dummy;
    while((dummy = fgetc(stdin)) != '\n')
    {
        printf("Flush");
    }
    // char newName[20];
    // fgets(newName, sizeof(newName), stdin);
}
void generateBooked(Rooms list[])//generate 2 randomly booked rooms
{
    for(int rooms = 0; rooms < 2; rooms++)
    {
        int ran = rand() % size; // chooses number between 0 - size
        
        while (list[ran].booked == Reserved) // if the room is already booked, then try again
        {
            ran = rand() % size;
        }
        list[ran].booked = Reserved; // will make the random room chosen as booked
    }
}
void staff_menu(Rooms list[])// staff menu
{
    printf("\nWhat would you like to do?\n\n");
    printf("(1) View Bookings \n(2) Add Booking \n(3) Remove Booking \n(#) Finished \n\nYour Choice: ");
    
    int answer = readInt(); // users answer
    switch(answer)
    {
        case SeeRooms: // if 1, then view the available rooms
            view_availability();
            break;
            
        case AddRoom: // if 2, allow user to add a room
            add_room(list);
            break;
            
        case RemoveRoom: // if 3, allow user to remove room
            remove_room(list);
            break;
    }
}
void guest_menu(Rooms list[])//guest list
{
    int answer;
    printf("\nWhat would you like to do?\n\n");
    printf("(1) Book A Room \n(2) Change Your Reservation \n(3) View Your Bookings \n(#) Finished \n\nYour Choice: ");
    
    answer = readInt(); // users input
    switch(answer)
    {
        case BookRoom: // if 1, allow user to book a room
            book_room(list);
            break;
        
        case ChangeRoom: // if 2, allow user to change their reservations
            change_rooms(list);
            break;
            
        case SeeBookings: // if 3, let user see the bookings they have made
            view_bookings(list);
            break;
    }
}
void view_availability()//view availability 
{
    printf("\nAt 'view_availability' function"); // used for testing
}
void add_room(Rooms list[])// add room
{
    if (quantity(list, Open) == 0) // checks to see if any rooms are open
    {
        printf("Sorry. All the rooms are already booked."); 
    }
    else
    {
        printf("\nWhat room would you like to add (1 - %d)? ", size);
        // the users answer minus one, so the answer is at the correct spot in list
        int ans = readInt() - 1; 
        
        while(check_booking(list, ans) != Open || ans >= size) 
        {
            if(check_booking(list, ans) == Reserved) // if the user's room is already booked
                printf("Sorry. That room is already booked. ");
        
            else // if the user's room number is out of range
                printf("Out of range.");
        
            ans = readInt() - 1;
        }
        list[ans].booked = Reserved; // will book the room
        printf("What name should the room be under? ");
        readChar(list[ans].name);
        fgets(list[ans].name, sizeof(list[ans].name), stdin); // will take the users name
        printf("\nSuccessfully added:\nName: %sRoom: %d\n", list[ans].name, ans + 1);
    }
    staff_menu(list);
}
void remove_room(Rooms list[])//remove room
{
    if (quantity(list, Reserved) == 0) // checks to make sure that there are rooms to be removed
    {
        printf("Sorry. All the rooms are already open.");
    }
    else
    {
        printf("\nWhat room would you like to remove from bookings (1 - %d)? ", size);
        int ans = readInt() - 1;
        
        while(check_booking(list, ans) != Reserved || ans >= size)
        {
            if(ans >= size) // if the user's room number is outside the range
                printf("Out of range.");
                
            else if(check_booking(list, ans) == Open) // if the user's room is already open
                printf("That room is still open. ");
                
            ans = readInt() - 1;
        }
        list[ans].booked = Open; // will change the room from reserved to open
        list[ans].name[0] = '\0'; // clears the name that was in room
    }
    staff_menu(list);
}
void book_room(Rooms list[])//book room
{
    if (quantity(list, Open) == 0)
    {
        printf("Sorry. All the rooms are already booked.");
    }
    else
    {
        printf("\nWhat room do you want to book (1 - %d)? ", size);
        int ans = readInt() - 1;
        
        while(check_booking(list, ans) != Open || ans >= size)
        {
            if(check_booking(list, ans) == Reserved) // if the room chosen is already booked
                printf("Sorry. That room is already booked. ");
            
            else if(ans >= size) // if the room chosen is out of range
                printf("Out of range.");

            else if (check_booking(list, ans) == User) // if the room chosen is already booked by the user
                printf("Sorry. You already booked that room. ");
        
            ans = readInt() - 1;
        }
        list[ans].booked = User; // makes the room booked by the user
        printf("What name do you want the room under? ");
        readChar(list[ans].name);
        fgets(list[ans].name, sizeof(list[ans].name), stdin); // will take the users name
        printf("\nSuccessfully booked you to: \nRoom: %d\nName: %s", ans + 1, list[ans].name);
    }
    guest_menu(list);
}
void change_rooms(Rooms list[]) //change rooms
{
    if (quantity(list, User) == 0) // checks to make sure that user has booked a room
        printf("\nYou do not have any bookings to change.\n");
    else
    {
        printf("\nWhich of these rooms do you want to change? [ ");
        
        // the for loop prints all the rooms that he has reserved
        for(int room = 0; room < size; room++) // check all the rooms
        {
            if(list[room].booked == User) // if the room is booked by the user
                printf("%d ", room + 1);
        }
        printf("] ");
        int ans = readInt() - 1;
        while (list[ans].booked != User || ans >= size)
        {
            if (ans >= size) // if the room is out of range
                printf("Out of range. ");
                
            else // if the room he wants to chnage is not booked by the user
                printf("You cannot change a room that you have not booked. ");
                
            ans = readInt() - 1;
        }
        list[ans].booked = Open; // chnages the user's room to open
        list[ans].name[0] = '\0'; // clears the name under that room
        
        printf("What room number would you like to move to? ");
        ans = readInt() - 1;
        while(check_booking(list, ans) != Open || ans >= size)
        {
            if(check_booking(list, ans) == Reserved) // if the room is already booked
                printf("Sorry. That room is already booked. ");
            
            else if(ans >= size) // if the room number is out of range
                printf("Out of range.");

            else if (check_booking(list, ans) == User) // if the room number is already booked by the user
                printf("Sorry. You already booked that room. ");
        
            ans = readInt() - 1;
        }
        list[ans].booked = User; // books the user to the room number
        printf("What name do you want the room under? ");
        readChar(list[ans].name);
        fgets(list[ans].name, sizeof(list[ans].name), stdin); // will take the users name
        printf("\nSuccessfully booked you to: \nRoom: %d\nName: %s", ans + 1, list[ans].name);
    }
    guest_menu(list);
}
void view_bookings(Rooms list[])//view bookings
{
    if (quantity(list, User) == 0) // checks to make sure that the user has booked a room
        printf("\nSorry. You have not made nay bookings yet. \n");
    else
    {
        printf("\nThese are the bookings you have made: \n\n");
        
        // for loop prints the room number and the name the user has booked
        for(int room = 0; room < size; room++)
        {
            if (check_booking(list, room) == User)
            {
                printf("%d   ", room + 1);
                printf("%s", list[room].name);
            }
        }
    }
    guest_menu(list);
    
}
int check_booking(Rooms list[], int room) //check bookings
{
    if(list[room].booked == Reserved)
        return Reserved;
    
    if(list[room].booked == User)
        return User;
    
    return Open;
}
int quantity(Rooms list[], int type) // how many of the type of bookings are there
{
    int amount = 0;
    
    // for loop checks every room to find the type of booking
    for(int room = 0; room < size; room++)
    {
        if(list[room].booked == type)
            amount++;
    }
    return amount;
}
void printArray(Rooms list[])//print array test
{
    printf("\n");
    for(int x = 0; x < 10; x++)
    {
        printf("%d ", list[x].booked);
        printf("%s", list[x].name);
    }
    printf("\n\n");
}
