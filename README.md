#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Donor {
    int id;
    char name[50];
    char bloodGroup[5];
    int age;
    char phone[15];
};

void addDonor() {
    FILE *fp = fopen("bloodbank.dat", "ab");
    struct Donor d;

    printf("\nEnter Donor ID: ");
    scanf("%d", &d.id);

    printf("Enter Name: ");
    scanf(" %[^\n]", d.name);

    printf("Enter Blood Group: ");
    scanf("%s", d.bloodGroup);

    printf("Enter Age: ");
    scanf("%d", &d.age);

    printf("Enter Phone Number: ");
    scanf("%s", d.phone);

    fwrite(&d, sizeof(d), 1, fp);
    fclose(fp);

    printf("\nDonor Added Successfully!\n");
}

void displayDonors() {
    FILE *fp = fopen("bloodbank.dat", "rb");
    struct Donor d;

    printf("\n--- Donor List ---\n");

    while (fread(&d, sizeof(d), 1, fp)) {
        printf("\nID: %d", d.id);
        printf("\nName: %s", d.name);
        printf("\nBlood Group: %s", d.bloodGroup);
        printf("\nAge: %d", d.age);
        printf("\nPhone: %s\n", d.phone);
        printf("------------------------");
    }

    fclose(fp);
}

void searchDonor() {
    FILE *fp = fopen("bloodbank.dat", "rb");
    struct Donor d;
    char bg[5];
    int found = 0;

    printf("\nEnter Blood Group to Search: ");
    scanf("%s", bg);

    while (fread(&d, sizeof(d), 1, fp)) {
        if (strcmp(d.bloodGroup, bg) == 0) {
            printf("\nID: %d", d.id);
            printf("\nName: %s", d.name);
            printf("\nAge: %d", d.age);
            printf("\nPhone: %s\n", d.phone);
            printf("------------------------");
            found = 1;
        }
    }

    if (!found)
        printf("\nNo donors found with this blood group!\n");

    fclose(fp);
}

void deleteDonor() {
    FILE *fp = fopen("bloodbank.dat", "rb");
    FILE *temp = fopen("temp.dat", "wb");
    struct Donor d;
    int id, found = 0;

    printf("\nEnter Donor ID to delete: ");
    scanf("%d", &id);

    while (fread(&d, sizeof(d), 1, fp)) {
        if (d.id != id) {
            fwrite(&d, sizeof(d), 1, temp);
        } else {
            found = 1;
        }
    }

    fclose(fp);
    fclose(temp);

    remove("bloodbank.dat");
    rename("temp.dat", "bloodbank.dat");

    if (found)
        printf("\nDonor Deleted Successfully!\n");
    else
        printf("\nDonor ID not found!\n");
}

int main() {
    int choice;

    while (1) {
        printf("\n====== Blood Bank Management System ======\n");
        printf("1. Add Donor\n");
        printf("2. Display Donors\n");
        printf("3. Search Donor by Blood Group\n");
        printf("4. Delete Donor\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addDonor(); break;
            case 2: displayDonors(); break;
            case 3: searchDonor(); break;
            case 4: deleteDonor(); break;
            case 5: exit(0);
            default: printf("Invalid Choice!\n");
        }
    }

    return 0;
}
