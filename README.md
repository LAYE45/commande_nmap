# commande_nmap
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <arpa/inet.h>
#include <unistd.h>

int main(int argc, char *argv[]) {
    if (argc != 2) {
        printf("Usage: %s <IP_ADDRESS>\n", argv[0]);
        return 1;
    }

    const char *ip_address = argv[1];
    struct sockaddr_in server;
    int sock, port;

    printf("Scanning TCP ports from 0 to 3000 on %s...\n", ip_address);

    for (port = 0; port <= 3000; port++) {
        sock = socket(AF_INET, SOCK_STREAM, 0);
        if (sock < 0) {
            perror("Socket creation failed");
            return 1;
        }

        server.sin_family = AF_INET;
        server.sin_port = htons(port);
        inet_pton(AF_INET, ip_address, &server.sin_addr);

        if (connect(sock, (struct sockaddr *)&server, sizeof(server)) == 0) {
            printf("Port %d is open\n", port);
        }

        close(sock);
    }

    printf("Scan completed.\n");
    return 0;
}
