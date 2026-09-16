
To set up C++ development in Visual Studio Code on macOS, you need to install the official C++ extension, configure a build task, and run your code.

Before starting, ensure you have already installed the compiler by running `xcode-select --install` in your Mac's Terminal.

## Step 1: Install the C/C++ Extension

1. Open Visual Studio Code.
2. Click the Extensions icon on the left Activity Bar (or press `Cmd + Shift + X`).
3. Search for "C++".
4. Install the official C/C++ extension pack released by Microsoft.

## Step 2: Open your Project Folder

1. Create a new folder on your Mac named `CppProject`.
2. In VS Code, go to File > Open Folder and select your `CppProject` folder.
3. Click the New File icon next to your folder name and create a file named `main.cpp`.
4. Paste the following sample code into the file and save it (`Cmd + S`):

```cpp
#include <iostream>

int main() {
    std::cout << "Hello World from VS Code!" << std::endl;
    return 0;
}
```

## Step 3: Run and Debug your Code

1. Open your `main.cpp` file in the editor window.
2. Click the Play icon (Run C/C++ File) in the top-right corner of the window.
3. From the drop-down menu that appears, select C/C++: clang++ build and debug active file.
4. VS Code will automatically open an integrated terminal at the bottom, build your file, and output:  
    `Hello World from VS Code!`

## Step 4: Managing Custom Builds (Optional)

If you need to pass specific compiler flags (like updating to C++20), you can modify the automatic configuration file:

- Press `Cmd + Shift + P` to open the Command Palette.
- Search for and select Tasks: Configure Default Build Task.
- Select C/C++: clang++ build active file. This creates a `.vscode/tasks.json` file.
- Inside the `"args"` array of that file, you can append flags like `"-std=c++20"` to update your language standard.

Would you like help with:

- Setting up a build system for multiple C++ files
- Configuring the CMake Tools extension
- Fixing an include path error or compiler warning

#CPP #Development #Gemini #VSCode
