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

void readInt();
void readChar();
void generateBooked(Rooms list[]);
void staff_menu();
void guest_menu();
void view_availability();
void add_room();
void remove_room();
void book_room();
void change_rooms();
void view_bookings();

int main()
{
    srand(time(NULL));
    Rooms list[size];
    generateBooked(list);
    printArray(list);
    int ans;
    printf("Are you a Staff (0) or Guest (1)? ");
    readInt(&ans);
    if (ans == Staff)
        staff_menu();
    else if (ans == Guest)
        guest_menu();
        
    return 0;
}
void readInt(int* ans)
{
    scanf("%d", ans);
}
void readChar(char string[])
{
    for (int x = 0; x < 40; x++) 
    { 
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
void staff_menu()
{
    int answer;
    printf("\nWhat would you like to do?\n\n");
    printf("(1) View Bookings \n(2) Add Booking \n(3) Remove Booking \n(#) Finished \n\nYour Choice: ");
    readInt(&answer);
    switch(answer)
    {
        case SeeRooms: 
            view_availability();
            break;
            
        case AddRoom:
            add_room();
            break;
            
        case RemoveRoom:
            remove_room();
            break;
    }
}
void guest_menu()
{
        int answer;
    printf("\nWhat would you like to do?\n\n");
    printf("(1) Book A Room \n(2) Change Your Reservation \n(3) View Your Bookings \n(#) Finished \n\nYour Choice: ");
    readInt(&answer);
    switch(answer)
    {
        case BookRoom: 
            book_room();
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
void add_room()
{
    printf("\nAt 'add_room' function"); // used for testing
}
void remove_room()
{
    printf("\nAt 'remove_room' function"); // used for testing
}
void book_room()
{
    printf("\nAt 'book_room' function"); // used for testing
}
void change_rooms()
{
    printf("\nAt 'change_rooms' function"); // used for testing
}
void view_bookings()
{
    printf("\nAt 'view_bookings' function"); // used for testing
}
void printArray(Rooms list[])
{
    for(int x = 0; x < 10; x++)
    {
        printf("%d ", list[x].booked);
    }
    printf("\n\n");
}
