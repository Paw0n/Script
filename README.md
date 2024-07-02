# Script
Dummy Python script

import platform
import subprocess
import os
import time

# Function to detect the OS
def get_os():
    return platform.system()

# Function to create a file
def create_file(filename, content=""):
    with open(filename, 'w') as file:
        file.write(content)
        print(f"File created: {filename}")

# Function to modify a file
def modify_file(file_path, text_to_append):
    try:
        if os.path.isfile(file_path):
            with open(file_path, 'a') as file:
                file.write(text_to_append + '\n')
            print(f"Text appended to {file_path}")
        else:
            print(f"File not found: {file_path}")
    except IOError as e:
        print(f"Error modifying file: {e}")

# Function to find and modify files in the specified directory
def find_and_modify_files(directory, text_to_append, file_extension=None):
    for file in os.listdir(directory):
        if file_extension and not file.endswith(file_extension):
            continue
        file_path = os.path.join(directory, file)
        if os.path.isfile(file_path):
            modify_file(file_path, text_to_append)

# Function to delete a file
def delete_file(filename):
    try:
        os.remove(filename)
        print(f"File deleted: {filename}")
    except Exception as e:
        print(f"Error deleting file: {e}")

def create_registry_key(key_path, key_name, value="Your Value"):
    os_type = get_os()
    if os_type != "Windows":
        print("Registry operations are only supported on Windows.")
        return

    try:
        import winreg
        with winreg.ConnectRegistry(None, winreg.HKEY_LOCAL_MACHINE) as hkey:
            with winreg.CreateKey(hkey, key_path) as reg_key:
                winreg.SetValueEx(reg_key, key_name, 0, winreg.REG_SZ, value)
                print(f"Registry key {key_name} created at {key_path}")
    except Exception as e:
        print(f"Error creating registry key: {e}")

# Main function to use the above functions
def main():
    if get_os() != "Windows":
        print("Unsupported operating system.")
        return

    directory = r"C:\Test"
    print(f"The current working directory is {directory}")

    filename = os.path.join(directory, "malicious_file.txt")

    create_file(filename)
    time.sleep(2)

    create_registry_key(r"SYSTEM\Setup\TestVkey", "VirusKey")

    # Modify files in the specified directory
    text_to_append = "test_file: malicious file"
    find_and_modify_files(directory, text_to_append)

    # Wait for 5 seconds before deleting the files
    time.sleep(5)
    delete_file(r"C:\Test\Credentials.txt")
    delete_file(r"C:\Test\Bank statements.txt")

if __name__ == "__main__":
    main()
