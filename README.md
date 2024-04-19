#include <stdio.h>
#include <stdbool.h>
#include <time.h>
#include <stdlib.h>
#define size 10

typedef struct
{
    char name[20];
    int booked;
} Rooms;

enum StaffOptions {SeeRooms = 1, AddRoom = 2, RemoveRoom = 3}; 
enum GuestOptions {BookRoom = 1, ChangeRoom = 2, SeeBookings = 3};
enum Options {Staff, Guest};
enum Bookings {Open, Reserved, User};

void printArray(Rooms list[]); // used for testing

int readInt(); // returns the user's integer intput
void readChar(); // reads a string from the user
void generateBooked(Rooms list[]); // generates random booked rooms
void staff_menu(Rooms list[]); 
void guest_menu(Rooms list[]);
void view_availability();
void add_room(Rooms list[]);
void remove_room();
void book_room(Rooms list[]);
void change_rooms();
void view_bookings();

int check_booking(Rooms list[], int room); // sees if the room is open, reserved, or booked by the user

int main()
{
    srand(time(NULL));
    Rooms list[size];
    generateBooked(list);
    printArray(list);
    printf("Are you a Staff (0) or Guest (1)? ");
    int ans = readInt();
    if (ans == Staff)
        staff_menu(list);
    else if (ans == Guest)
        guest_menu(list);
        
    return 0;
}
int readInt()
{
    int ans;
    scanf("%d", &ans);
    return ans;
}
void readChar(char string[])
{
    for (int x = 0; x < 20; x++) 
    { 
        printf("WHAT\n");
        scanf("%c", (string + x) ); 
        if ( *(string + x) == '\n' ) 
        {
            break;
        }
    }
}
void generateBooked(Rooms list[])
{
    for(int x = 0; x < 2; x++)
    {
        int ran = rand() % size;
        while (list[ran].booked == Reserved)
        {
            ran = rand() % size;
        }
        list[ran].booked = Reserved;
    }
}
void staff_menu(Rooms list[])
{
    printf("\nWhat would you like to do?\n\n");
    printf("(1) View Bookings \n(2) Add Booking \n(3) Remove Booking \n(#) Finished \n\nYour Choice: ");
    int answer = readInt();
    switch(answer)
    {
        case SeeRooms: 
            view_availability();
            break;
            
        case AddRoom:
            add_room(list);
            break;
            
        case RemoveRoom:
            remove_room();
            break;
    }
}
void guest_menu(Rooms list[])
{
        int answer;
    printf("\nWhat would you like to do?\n\n");
    printf("(1) Book A Room \n(2) Change Your Reservation \n(3) View Your Bookings \n(#) Finished \n\nYour Choice: ");
    readInt(&answer);
    switch(answer)
    {
        case BookRoom: 
            book_room(list);
            break;
        
        case ChangeRoom:
            change_rooms();
            break;
            
        case SeeBookings:
            view_bookings();
            break;
    }
}
void view_availability()
{
    printf("\nAt 'view_availability' function"); // used for testing
}
void add_room(Rooms list[])
{
    printf("What room would you like to add? ");
    int ans = readInt() - 1;
    while(check_booking(list, ans) != Open || ans > size)
    {
        if(check_booking(list, ans) == Reserved)
            printf("Sorry. That room is already booked. ");
        
        else
            printf("Out of range.");
        
        ans = readInt() - 1;
    }
    list[ans].booked = Reserved;
    /* 
    printf("What name should the room be under? \n");
    readChar(list[ans].name);
    printf("%s \n", list[ans].name);
    */
    printf("\nSuccessfully added you to room %d \n", ans + 1);
    staff_menu(list);
}
void remove_room()
{
    printf("\nAt 'remove_room' function"); // used for testing
}
void book_room(Rooms list[])
{
    printf("What room do you want to book (1 - %d)? ", size);
    int ans = readInt() - 1;
    while(check_booking(list, ans) != Open || ans > size)
    {
        if(check_booking(list, ans) == Reserved)
            printf("Sorry. That room is already booked. ");

        else if (check_booking(list, ans) == User)
            printf("Sorry. You already booked that room. ");
        
        else
            printf("Out of range.");
        
        ans = readInt() - 1;
    }
    list[ans].booked = User;
    printf("\nSuccessfully added you to room %d \n", ans + 1);
    guest_menu(list);
}
void change_rooms()
{
    printf("\nAt 'change_rooms' function"); // used for testing
}
void view_bookings()
{
    printf("\nAt 'view_bookings' function"); // used for testing
}
int check_booking(Rooms list[], int room)
{
    if(list[room].booked == Reserved)
        return Reserved;
    
    if(list[room].booked == User)
        return User;
    
    return Open;
}
void printArray(Rooms list[])
{
    printf("\n");
    for(int x = 0; x < 10; x++)
    {
        printf("%d ", list[x].booked);
    }
    printf("\n\n");
}
