# Data-Structure-IA-2
watch-list / read-list tracker using linked-lists (c)

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Watchlist
{
  char data[100];
  struct Watchlist *next;
} Watchlist;

typedef struct Readlist
{
  char data[100];
  struct Readlist *next;
} Readlist;

typedef struct DeletedList
{
  char data[100];
  struct DeletedList *next;
} DeletedList;

Watchlist *createNode(const char *data)
{
  Watchlist *newNode = (Watchlist *)malloc(sizeof(Watchlist));
  if (newNode == NULL)
  {
    printf("Memory allocation failed.\n");
    return NULL;
  }
  strcpy(newNode->data, data);
  newNode->next = NULL;
  return newNode;
}

Readlist *createReadNode(const char *data)
{
  Readlist *newNode = (Readlist *)malloc(sizeof(Readlist));
  if (newNode == NULL)
  {
    printf("Memory allocation failed.\n");
    return NULL;
  }
  strcpy(newNode->data, data);
  newNode->next = NULL;
  return newNode;
}

DeletedList *createDeletedNode(const char *data)
{
  DeletedList *newNode = (DeletedList *)malloc(sizeof(DeletedList));
  if (newNode == NULL)
  {
    printf("Memory allocation failed.\n");
    return NULL;
  }
  strcpy(newNode->data, data);
  newNode->next = NULL;
  return newNode;
}

void addString(Watchlist **head, const char *data)
{
  Watchlist *newNode = createNode(data);
  if (newNode == NULL)
  {
    return;
  }

  if (*head == NULL)
  {
    *head = newNode;
  }
  else
  {
    Watchlist *current = *head;
    while (current->next != NULL)
    {
      current = current->next;
    }
    current->next = newNode;
  }
  printf("\nAdded \"%s\" to the watchlist.\n", data);
}

void addReadString(Readlist **head, const char *data)
{
  Readlist *newNode = createReadNode(data);
  if (newNode == NULL)
  {
    return;
  }

  if (*head == NULL)
  {
    *head = newNode;
  }
  else
  {
    Readlist *current = *head;
    while (current->next != NULL)
    {
      current = current->next;
    }
    current->next = newNode;
  }
  printf("\nAdded \"%s\" to the readlist.\n", data);
}

void displayList(Watchlist *head)
{
  if (head == NULL)
  {
    printf("\nThe watchlist is empty.\n");
    return;
  }

  printf("\nList of strings in the watchlist:\n");
  Watchlist *current = head;
  while (current != NULL)
  {
    printf("%s\n", current->data);
    current = current->next;
  }
}

void displayReadList(Readlist *head)
{
  if (head == NULL)
  {
    printf("\nThe readlist is empty.\n");
    return;
  }

  printf("\nList of strings in the readlist:\n");
  Readlist *current = head;
  while (current != NULL)
  {
    printf("%s\n", current->data);
    current = current->next;
  }
}

void freeList(Watchlist **head)
{
  Watchlist *current = *head;
  while (current != NULL)
  {
    Watchlist *temp = current;
    current = current->next;
    free(temp);
  }
  *head = NULL;
}

void freeReadList(Readlist **head)
{
  Readlist *current = *head;
  while (current != NULL)
  {
    Readlist *temp = current;
    current = current->next;
    free(temp);
  }
  *head = NULL;
}

void deleteString(Watchlist **head, DeletedList **deletedHead, const char *data)
{
  Watchlist *current = *head;
  Watchlist *prev = NULL;

  while (current != NULL)
  {
    if (strcmp(current->data, data) == 0)
    {
      if (prev == NULL)
      {
        *head = current->next;
      }
      else
      {
        prev->next = current->next;
      }
      DeletedList *deletedNode = createDeletedNode(data);
      if (deletedNode != NULL)
      {
        deletedNode->next = *deletedHead;
        *deletedHead = deletedNode;
      }
      free(current);
      printf("\nRemoved %s from the watchlist.\n", data);
      return;
    }
    prev = current;
    current = current->next;
  }

  printf("%s not found in the watchlist.\n", data);
}

void deleteReadString(Readlist **head, DeletedList **deletedHead, const char *data)
{
  Readlist *current = *head;
  Readlist *prev = NULL;

  while (current != NULL)
  {
    if (strcmp(current->data, data) == 0)
    {
      if (prev == NULL)
      {
        *head = current->next;
      }
      else
      {
        prev->next = current->next;
      }
      DeletedList *deletedNode = createDeletedNode(data);
      if (deletedNode != NULL)
      {
        deletedNode->next = *deletedHead;
        *deletedHead = deletedNode;
      }
      free(current);
      printf("\nRemoved %s from the readlist.\n", data);
      return;
    }
    prev = current;
    current = current->next;
  }

  printf("%s not found in the readlist.\n", data);
}

void displayDeletedList(DeletedList *deletedHead)
{
  if (deletedHead == NULL)
  {
    printf("\nThe deleted list is empty.\n");
    return;
  }

  printf("\nList of deleted strings:\n");
  DeletedList *current = deletedHead;
  while (current != NULL)
  {
    printf("%s\n", current->data);
    current = current->next;
  }
}

int main()
{
  int choose;
  printf("ENTER: \n1] WATCHLIST      2] READLIST      3] to exit\n");
  scanf("%d", &choose);

  if (choose == 1)
  {
    Watchlist *stringList = NULL;
    DeletedList *deletedList = NULL;
    char input[100];
    char val[100];
    char choice;

    do
    {
      printf("\n\nEdit Movie Watch-List: \n");
      printf("\n1. Add Movie\n");
      printf("2. Display Watch-List\n");
      printf("3. Remove movie (completed watching)\n");
      printf("4. Display Completed-Movie List\n");
      printf("5. Empty List\n");
      printf("6. Exit\n");
      printf("\nEnter your choice : ");
      scanf(" %c", &choice);

      switch (choice)
      {
      case '1':
        printf("\nEnter a string: ");
        if (scanf(" %[^\n]", input) != 1)
        {
          printf("\nInvalid input. Please try again.\n");
          while (getchar() != '\n')
            ;
          continue;
        }
        addString(&stringList, input);
        break;

      case '2':
        displayList(stringList);
        break;

      case '3':
        printf("\nEnter the value you want to delete: ");
        if (scanf(" %[^\n]", val) != 1)
        {
          printf("\nInvalid input. Please try again.\n");
          while (getchar() != '\n')
            ;
          continue;
        }
        deleteString(&stringList, &deletedList, val);
        break;

      case '4':
        displayDeletedList(deletedList);
        break;

      case '5':
        freeList(&stringList);
        break;

      case '6':
        break;

      default:
        printf("\nInvalid choice. Please try again.\n");
      }
    } while (choice != '6');
  }
  else if (choose == 2)
  {
    Readlist *stringList = NULL;
    DeletedList *deletedList = NULL;
    char input[100];
    char val[100];
    char choice;

    do
    {
      printf("\n\nEdit Book Read-List: \n");
      printf("\n1. Add Book\n");
      printf("2. Display Read-List\n");
      printf("3. Remove book (completed reading)\n");
      printf("4. Display Completed-Book List\n");
      printf("5. Empty List\n");
      printf("6. Exit\n");
      printf("\nEnter your choice : ");
      scanf(" %c", &choice);

      switch (choice)
      {
      case '1':
        printf("\nEnter a string: ");
        if (scanf(" %[^\n]", input) != 1)
        {
          printf("\nInvalid input. Please try again.\n");
          while (getchar() != '\n')
            ;
          continue;
        }
        addReadString(&stringList, input);
        break;

      case '2':
        displayReadList(stringList);
        break;

      case '3':
        printf("\nEnter the value you want to delete: ");
        if (scanf(" %[^\n]", val) != 1)
        {
          printf("\nInvalid input. Please try again.\n");
          while (getchar() != '\n')
            ;
          continue;
        }
        deleteReadString(&stringList, &deletedList, val);
        break;

      case '4':
        displayDeletedList(deletedList);
        break;

      case '5':
        freeReadList(&stringList);
        break;

      case '6':
        break;

      default:
        printf("\nInvalid choice. Please try again.\n");
      }
    } while (choice != '6');
  }

  return 0;
}
