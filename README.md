import os
import shutil


def synchronize_folders(source, replica):
    # Ensure both folders exist
    if not os.path.exists(source) or not os.path.exists(replica):
        print("Source or replica folder does not exist.")
        return

    # Walk through the source folder
    for root, dirs, files in os.walk(source):
        # Create corresponding directory structure in replica folder
        relative_path = os.path.relpath(root, source)
        replica_root = os.path.join(replica, relative_path)
        os.makedirs(replica_root, exist_ok=True)

        # Synchronize files
        for file in files:
            source_file = os.path.join(root, file)
            replica_file = os.path.join(replica_root, file)
            # Copy if file does not exist or is different
            if not os.path.exists(replica_file) or os.stat(source_file).st_mtime - os.stat(replica_file).st_mtime > 1:
                shutil.copy2(source_file, replica_file)
                print(f"Copied: {source_file} to {replica_file}")

    print("Synchronization complete.")


# Example usage:
source_folder = "/path/to/source"
replica_folder = "/path/to/replica"

synchronize_folders(source_folder, replica_folder)
