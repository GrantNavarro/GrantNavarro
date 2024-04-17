
#include <stdio.h>
#include <stdbool.h>
#include <time.h>
#include <stdlib.h>

typedef struct
{
    char name[10];
    int taken;
    bool user;
} Rooms;

enum StaffMenu {SeeRooms = 1, AddRoom = 2, RemoveRoom = 3};
enum GuestMenu {BookRoom = 1, ChangeRoom = 2, SeeBookings = 3};
enum Options {Staff, Guest};
enum Booked {Open, Reserved};

void generateBooked(Rooms list[]);
void staff_menu();
void guest_menu();

int main()
{
    srand(time(NULL));
    Rooms list[10];
    generateBooked(list);
    int ans;
    printf("Are you a Staff (0) or Guest (1) ? ");
    scanf("%d", &ans);
    if (ans == Staff)
        staff_menu();
    else if (ans == Guest)
        guest_menu();
        
    return 0;
}
void generateBooked(Rooms list[])
{
    for(int x = 0; x < 2; x++)
    {
        int ran = rand() % 10;
        while (list[ran].taken == Reserved)
        {
            ran = rand() % 10;
        }
        list[ran].taken = 1;
    }
}
void staff_menu()
{
    printf("You're staff");
}
void guest_menu()
{
    printf("You're guest");
}
