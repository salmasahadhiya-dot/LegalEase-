#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <stdlib.h>

#define MAX_TEXT 10000
#define MAX_WORD 100

// Legal Dictionary for Simplification
struct Dictionary {
    char legal_word[50];
    char simple_word[50];
} dict[] = {
    {"indemnify", "protect from loss"},
    {"hold harmless", "not responsible"},
    {"notwithstanding", "even if"},
    {"herein", "in this document"},
    {"hereinafter", "from now on"},
    {"aforementioned", "mentioned before"},
    {"jurisdiction", "legal power"},
    {"liability", "responsibility"},
    {"breach", "breaking of rule"},
    {"termination", "ending"},
    {"confidentiality", "keeping secret"},
    {"obligation", "duty"},
    {"commencement", "start"},
    {"null and void", "cancelled"},
};

int dict_size = sizeof(dict) / sizeof(dict[0]);

// Function to convert string to lower for comparison
void toLowerCase(char *str) {
    for (int i = 0; str[i]; i++) {
        str[i] = tolower(str[i]);
    }
}

// 1. Simplify Document
void simplifyDocument(char *text) {
    char lower_text[MAX_TEXT];
    strcpy(lower_text, text);
    toLowerCase(lower_text);

    printf("\n--- Simplified Document ---\n");
    // Simple replacement logic
    for (int i = 0; i < dict_size; i++) {
        char legal_lower[50];
        strcpy(legal_lower, dict[i].legal_word);
        toLowerCase(legal_lower);

        if (strstr(lower_text, legal_lower)!= NULL) {
            printf("'%s' -> '%s'\n", dict[i].legal_word, dict[i].simple_word);
        }
    }

    // Basic jargon free text
    printf("\n[Simplified Text Preview]\n");
    printf("%s\n", text);
    printf("\nNote: The above highlighted words are complex legal jargons. Their simple meanings are shown above.\n");
}

// 2. Risky Clause Detector
void detectRiskyClauses(char *text) {
    char lower_text[MAX_TEXT];
    strcpy(lower_text, text);
    toLowerCase(lower_text);

    char *risky_keywords[] = {"indemnify", "liability", "penalty", "termination", "breach", "lawsuit", "confidentiality", "non-compete"};
    int risky_count = 8;
    int found = 0;

    printf("\n--- Risk Analysis Report ---\n");
    for (int i = 0; i < risky_count; i++) {
        if (strstr(lower_text, risky_keywords[i])!= NULL) {
            printf("[HIGH RISK] Found clause: '%s' - Please review carefully!\n", risky_keywords[i]);
            found = 1;
        }
    }
    if (!found) {
        printf("No high-risk clauses found. Document looks safe.\n");
    }
}

// 3. Generate Summary
void generateSummary(char *text) {
    int sentences = 0, words = 0;
    for (int i = 0; text[i]!= '\0'; i++) {
        if (text[i] == '.' || text[i] == '!' || text[i] == '?') sentences++;
        if (text[i] == ' ' || text[i] == '\n') words++;
    }

    printf("\n--- Document Summary ---\n");
    printf("Total Words: %d\n", words);
    printf("Total Sentences: %d\n", sentences);
    printf("Summary: This document contains %d main points. ", sentences);
    if (strstr(text, "Party")!= NULL || strstr(text, "party")!= NULL) {
        printf("It is an agreement between two parties. ");
    }
    printf("Please read all risky clauses before signing.\n");
}

// 4. Extract Keywords
void extractKeywords(char *text) {
    printf("\n--- Extracted Information ---\n");
    // This is a basic demo - you can improve with file parsing
    if (strstr(text, "Rs")!= NULL || strstr(text, "$")!= NULL) {
        printf("-> Contains Financial Information (Payment/Amount)\n");
    }
    if (strstr(text, "date")!= NULL || strstr(text, "Date")!= NULL) {
        printf("-> Contains Dates / Deadlines\n");
    }
    if (strstr(text, "Party")!= NULL) {
        printf("-> Contains Party Details\n");
    }
    printf("-> Document Type: Legal Contract/Agreement\n");
}

int main() {
    char text[MAX_TEXT] = "";
    int choice;
    FILE *file;

    printf("========================================\n");
    printf(" Welcome to LegalEase - C Version\n");
    printf("========================================\n");

    // Try to read from input.txt
    file = fopen("input.txt", "r");
    if (file == NULL) {
        printf("input.txt not found! Please enter your legal text manually.\n");
        printf("Enter your legal document (type END in new line to finish):\n");
        char line[500];
        while (1) {
            fgets(line, sizeof(line), stdin);
            if (strcmp(line, "END\n") == 0) break;
            strcat(text, line);
        }
    } else {
        char ch;
        int i = 0;
        while ((ch = fgetc(file))!= EOF && i < MAX_TEXT -1) {
            text[i++] = ch;
        }
        text[i] = '\0';
        fclose(file);
        printf("Document loaded from input.txt successfully!\n");
    }

    if (strlen(text) == 0) {
        printf("No text entered. Exiting.\n");
        return 0;
    }

    do {
        printf("\n===== LegalEase Menu =====\n");
        printf("1. Simplify Document\n");
        printf("2. Find Risky Clauses\n");
        printf("3. Generate Summary\n");
        printf("4. Extract Keywords\n");
        printf("5. View Full Document\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar(); // clear buffer

        switch (choice) {
            case 1: simplifyDocument(text); break;
            case 2: detectRiskyClauses(text); break;
            case 3: generateSummary(text); break;
            case 4: extractKeywords(text); break;
            case 5: printf("\n--- Full Document ---\n%s\n", text); break;
            case 6: printf("Thank you for using LegalEase!\n"); break;
            default: printf("Invalid choice!\n");
        }
    } while (choice!= 6);

    return 0;
}
name: Salma Sahadhiya
