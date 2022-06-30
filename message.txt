#include <stdio.h>

#include <conio.h>

#include <ctype.h>

#include <windows.h>

#include <stdlib.h>

struct customer

{

    char phone[20];

    char customer_name[50];

    float BillAmount;

} s;

void add();

void view_record();

void modify_records();

void delete_records();

void search_records();

void pay();

char get;

int main()

{
    int pass;

    int phone;

    char c;

    system("cls");

    printf("\n\t\t**************************************************************");

    printf("\n\t\t-WELCOME TO THE BILLING MANAGEMENT SYSTEM(For Telecom Sector)-");

    printf("\n\t\t**************************************************************");

    Sleep(3000);

    getch();

    system("cls");

    while (1)

    {

        system("cls");

        printf("\n A : for adding new records\n");

        printf("\n L : for viewing list of records\n");

        printf("\n M : for modifying records\n");

        printf("\n P : for payment\n");

        printf("\n S : for searching records\n");

        printf("\n E : for exit\n");

        c = getche();

        c = toupper(c);

        switch (c)

        {

        case 'P':

            pay();

            break;

        case 'A':

            add();

            break;

        case 'L':

            view_record();

            break;

        case 'M':

            modify_records();

            break;

        case 'S':

            search_records();

            break;

        case 'E':

            system("cls");

            printf("\n\n\t\tTHANK YOU");

            printf("\n\t Using this portal for your data maintance");

            Sleep(3000);

            exit(0);

            break;

        default:

            system("cls");

            printf("Incorrect Choice");

            printf("\n Press Any key to continue");

            getch();
        }
    }
}

void add()

{

    FILE *f;

    char test;

    f = fopen("data.ojs", "ab+"); // here file data.ojs : file name .which contain datas //

    if (f == 0)

    {
        f = fopen("data.ojs", "wb+");

        system("cls");

        printf("please wait while we ready your system");

        printf("\n Press any key to continue");

        getch();
    }

    while (1)

    {

        system("cls");

        printf("\n phone number:");

        scanf("%s", &s.phone);

        printf("\n Enter customer Name:");

        scanf("%s[n]", &s.customer_name);

        printf("\n Enter Bill Amount:");

        scanf("%f", &s.BillAmount);

        fwrite(&s, sizeof(s), 1, f);

        fflush(stdin);

        system("cls");

        printf("Your record successfully added");

        printf("\n Press esc key to exit, any other key to add other record:");

        test = getche();

        if (test == 27) // 27 is ASCII value of ESC key //

            break;
    }

    fclose(f); // file close //
}

void view_record()

{

    FILE *f;

    int i;

    if ((f = fopen("data.ojs", "rb")) == NULL)

        exit(0);

    system("cls");

    printf("Phone Number\t\tUser customer Name\t\t Bill Amount\n");

    for (i = 0; i < 80; i++) // Here 80 is limit of record //

        printf("-");

    while (fread(&s, sizeof(s), 1, f) == 1)

    {

        printf("\n %-10s\t\t %-20s\t\tRs. %.2f /-", s.phone, s.customer_name, s.BillAmount);
    }

    printf("\n");

    for (i = 0; i < 80; i++)

        printf("-");

    fclose(f);

    getch();
}

void search_records()

{

    FILE *f;

    char search_phone[20];

    int flag = 1;

    f = fopen("data.ojs", "rb+");

    if (f == 0)

        exit(0);

    fflush(stdin);

    system("cls");

    printf("Enter Phone Number to search in our database: ");

    scanf("%s", search_phone);

    while (fread(&s, sizeof(s), 1, f) == 1)

    {

        if (strcmp(s.phone, search_phone) == 0)

        {
            system("cls");

            printf(" Record Founded !Details is given below.");

            printf("\n\nphone: %s\ncustomer_name: %s\nBillAmount: Rs.% 0.2f\n", s.phone, s.customer_name, s.BillAmount);

            flag = 0;

            break;
        }

        else if (flag == 1)

        {
            system("cls");

            printf("Phone Number %s Not found in database ", search_phone);
        }
    }

    getch();

    fclose(f);
}

void modify_records()

{

    FILE *f;

    char change_phone[20];

    long int size = sizeof(s);

    if ((f = fopen("data.ojs", "rb+")) == NULL)

        exit(0);

    system("cls");

    printf("Enter phone number of the customer to modify:");

    scanf("%s[n]", change_phone);

    fflush(stdin);

    while (fread(&s, sizeof(s), 1, f) == 1)

    {

        if (strcmp(s.phone, change_phone) == 0)

        {

            system("cls");

            printf("\n Enter phone number:");

            scanf("%s", &s.phone);

            printf("\n Enter customer name: ");

            fflush(stdin);

            scanf("%s[n]", &s.customer_name);

            printf("\n Enter Bill Amount:");

            scanf("%f", &s.BillAmount);

            fseek(f, -size, SEEK_CUR);

            fwrite(&s, sizeof(s), 1, f);

            break;
        }
    }

    fclose(f);
}

void pay()

{

    FILE *f;

    char pay_phone[20];

    long int size = sizeof(s);

    float PaidAmount;

    int i;

    if ((f = fopen("data.ojs", "rb+")) == NULL)

        exit(0);

    system("cls");

    printf("Enter phone number of the customer to pay: ");

    scanf("%s[n]", pay_phone);

    fflush(stdin);

    while (fread(&s, sizeof(s), 1, f) == 1)

    {

        if (strcmp(s.phone, pay_phone) == 0)

        {

            system("cls");

            printf("\n Phone No.: %s", s.phone);

            printf("\n Customer Name: %s", s.customer_name);

            printf("\n Current Bill Amount: %.2f", s.BillAmount);

            printf("\n");

            for (i = 0; i < 79; i++)

                printf("-");

            printf("\n\nEnter Bill Amount of pay :");

            fflush(stdin);

            scanf("%f", &PaidAmount);

            s.BillAmount = s.BillAmount - PaidAmount;

            fseek(f, -size, SEEK_CUR);

            fwrite(&s, sizeof(s), 1, f);

            break;
        }
    }

    system("cls");

    printf("THANK YOU %s ! FOR Paying Your Bill On Time ", s.customer_name);

    getch();

    fclose(f);
}