# escape=`

ARG REPO=mcr.microsoft.com/dotnet/core/runtime

# Installer image
FROM mcr.microsoft.com/windows/servercore:1809 AS installer

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Install ASP.NET Core Runtime
ENV ASPNETCORE_VERSION 3.1.0-preview1.19503.31

RUN Invoke-WebRequest -OutFile aspnetcore.zip https://dotnetcli.blob.core.windows.net/dotnet/aspnetcore/Runtime/$Env:ASPNETCORE_VERSION/aspnetcore-runtime-$Env:ASPNETCORE_VERSION-win-x64.zip; `
    $aspnetcore_sha512 = '79f7ffdbb37ea6103472fac7ad5211c7b74fd3dd4afbfcf9951a8788a973b6f3128c005f43c10ba6d93ff10c0685c440a8fd4907383e49f66dcd6255ac22a673'; `
    if ((Get-FileHash aspnetcore.zip -Algorithm sha512).Hash -ne $aspnetcore_sha512) { `
        Write-Host 'CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive aspnetcore.zip -DestinationPath dotnet; `
    Remove-Item -Force aspnetcore.zip


# Runtime image
FROM $REPO:3.1-nanoserver-1809

COPY --from=installer ["/dotnet/shared/Microsoft.AspNetCore.App", "/Program Files/dotnet/shared/Microsoft.AspNetCore.App"]