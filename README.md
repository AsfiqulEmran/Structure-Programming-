#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define SHIFT 3    
#define MAX_PATH 260
#define MAX_PASS 128

// Caesar Cipher Encryption (only alphabets)
void caesarEncryptFile(const char *inputFile, const char *outputFile) {
    FILE *in = fopen(inputFile, "r");
    FILE *out = fopen(outputFile, "w");

    if (!in || !out) {
        printf("Error opening file.\n");
        return;
    }

    char ch;
    while ((ch = fgetc(in)) != EOF) {
        if (ch >= 'a' && ch <= 'z')
            ch = ((ch - 'a' + SHIFT) % 26) + 'a';
        else if (ch >= 'A' && ch <= 'Z')
            ch = ((ch - 'A' + SHIFT) % 26) + 'A';

        fputc(ch, out);
    }

    fclose(in);
    fclose(out);
    printf("Caesar encryption done. Output: %s\n", outputFile);
}

// Caesar Cipher Decryption
void caesarDecryptFile(const char *inputFile, const char *outputFile) {
    FILE *in = fopen(inputFile, "r");
    FILE *out = fopen(outputFile, "w");

    if (!in || !out) {
        printf("Error opening file.\n");
        return;
    }

    char ch;
    while ((ch = fgetc(in)) != EOF) {
        if (ch >= 'a' && ch <= 'z')
            ch = ((ch - 'a' - SHIFT + 26) % 26) + 'a';
        else if (ch >= 'A' && ch <= 'Z')
            ch = ((ch - 'A' - SHIFT + 26) % 26) + 'A';

        fputc(ch, out);
    }

    fclose(in);
    fclose(out);
    printf("Caesar decryption done. Output: %s\n", outputFile);
}

// XOR Encryption/Decryption (all characters)
void xorEncryptDecryptFile(const char *inputFile, const char *outputFile, const char *password) {
    FILE *in = fopen(inputFile, "rb");
    FILE *out = fopen(outputFile, "wb");

    if (!in || !out) {
        printf("Error opening file.\n");
        return;
    }

    int passLen = strlen(password);
    int i = 0;
    int ch;

    while ((ch = fgetc(in)) != EOF) {
        ch = ch ^ password[i % passLen];
        fputc(ch, out);
        i++;
    }

    fclose(in);
    fclose(out);
    printf("XOR operation complete. Output: %s\n", outputFile);
}

// Menu
int main() {
    int choice;
    char inputFile[MAX_PATH], outputFile[MAX_PATH], password[MAX_PASS];

    printf("==== File Encryption & Decryption Utility ====\n");

    while (1) {
        printf("\nMenu:\n");
        printf("1. Caesar Encrypt\n");
        printf("2. Caesar Decrypt\n");
        printf("3. XOR Encrypt/Decrypt with Password\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar(); // consume newline

        if (choice == 4) {
            printf("Exiting program. Goodbye!\n");
            break;
        }

        printf("Enter input file name: ");
        fgets(inputFile, sizeof(inputFile), stdin);
        inputFile[strcspn(inputFile, "\n")] = 0;

        printf("Enter output file name: ");
        fgets(outputFile, sizeof(outputFile), stdin);
        outputFile[strcspn(outputFile, "\n")] = 0;

        if (choice == 3) {
            printf("Enter password: ");
            fgets(password, sizeof(password), stdin);
            password[strcspn(password, "\n")] = 0;
        }

        switch (choice) {
            case 1:
                caesarEncryptFile(inputFile, outputFile);
                break;
            case 2:
                caesarDecryptFile(inputFile, outputFile);
                break;
            case 3:
                xorEncryptDecryptFile(inputFile, outputFile, password);
                break;
            default:
                printf("Invalid choice. Try again.\n");
        }
    }

    return 0;
}
