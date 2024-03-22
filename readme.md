Recently, I figured out how to install bitmap fonts on to NixOS. Orginally, this is something that I was very interested in trying to do when I was messing with Arch a few years ago. But I dumb so


# To Install

The command that I found was orginally in this post [here](https://discourse.nixos.org/t/how-to-install-bitmap-fonts/3147), where adding this to `/etc/nixos/configuration.nix`, with some modification to support newer language changes:

```nixos
{pkgs, ...}: {
  fonts.packages = [ "${pkgs.fetchFromGitHub {
    owner = "Tecate";
    repo = "bitmap-fonts";
    rev = "5c101c91bf2ed0039aad02f9bf76ddb2018b1f21";
    sha256 = "0s119zln3yrhhscfwkjncj72cr68694643009aam63s2ng4hsmfl";
  }}/bitmap" ];
  fonts.fontconfig.allowBitmaps = true;
}
```
would install the bitmap fonts found [on this Github repo](https://github.com/Tecate/bitmap-fonts)


The `owner` and `repo` group should be obvious, but the `rev` and the `sha256` are not (at least to me). We can look at GPT response to help do this:

To obtain the `rev` and `sha256` for use in a `fetchFromGitHub` call in NixOS, you need to follow a few steps. These values are essential for ensuring the reproducibility and integrity of the fetched resource.

1. **Finding the `rev`**:
   - The `rev` is the specific commit hash or tag you want to fetch from a GitHub repository. To find this, you can navigate to the GitHub repository web page, browse the commit history or releases, and choose a specific commit or release that you want to use. The commit hash is usually a 40-character hexadecimal string displayed on the commit's page. Tags for releases can also be used and are found on the repository's releases page.

2. **Calculating the `sha256`**:
   - The `sha256` is a hash value used to verify the integrity of the downloaded resource, ensuring it has not been altered. To obtain this hash, you can use the `nix-prefetch-url` command for URLs or, more specifically for GitHub repositories, `nix-prefetch-github`.

Here's how to calculate the `sha256` for a GitHub repository:

- **Using `nix-prefetch-github`** (the preferred method for GitHub repositories):
  - This tool is part of the `nixpkgs` and can be used specifically for fetching and calculating the SHA256 of GitHub repositories. It's more convenient for GitHub because it automatically handles the API calls and archive fetching.
  - To use `nix-prefetch-github`, you would run a command in your terminal like:
    ```sh
    nix-prefetch-github --owner <owner> --repo <repo> --rev <commit_or_tag>
    ```
  - Replace `<owner>` with the GitHub username or organization name, `<repo>` with the repository name, and `<commit_or_tag>` with the commit hash or tag you are interested in.
  - The command will output information including the `sha256` hash.

- **Using `nix-prefetch-url` for a direct URL**:
  - If you have a direct URL to the archive of the specific revision you want to fetch, you can also use `nix-prefetch-url` by providing the URL to it directly. This is a more general tool that can be used for any URL, not just GitHub repositories.
  - The command format is:
    ```sh
    nix-prefetch-url --url <archive_url>
    ```
  - After running, it outputs the `sha256` hash of the file at the provided URL.

The `rev` and `sha256` are then used in your Nix expressions to refer to a specific state of the repository content securely and reproducibly. This approach ensures that every time your Nix expression is evaluated, it fetches the exact same version of the code or resource, maintaining the reproducibility of your builds and configurations.
