# escape=`
FROM microsoft/dotnet-framework:4.7.2-sdk-windowsservercore-ltsc2019 AS builder

WORKDIR C:\src
COPY src\SignUp.sln .
COPY src\SignUp.Core\SignUp.Core.csproj .\SignUp.Core\
COPY src\SignUp.Entities\SignUp.Entities.csproj .\SignUp.Entities\
COPY src\SignUp.Messaging\SignUp.Messaging.csproj .\SignUp.Messaging\
COPY src\SignUp.Model\SignUp.Model.csproj .\SignUp.Model\
COPY src\SignUp.Model\packages.config .\SignUp.Model\
COPY src\SignUp.MessageHandlers.SaveProspect\SignUp.MessageHandlers.SaveProspect.csproj .\SignUp.MessageHandlers.SaveProspect\
COPY src\SignUp.MessageHandlers.SaveProspect\packages.config .\SignUp.MessageHandlers.SaveProspect\
RUN nuget restore .\SignUp.sln

COPY src C:\src
RUN msbuild SignUp.MessageHandlers.SaveProspect\SignUp.MessageHandlers.SaveProspect.csproj /p:OutputPath=c:\out\save-prospect\SaveProspectHandler

# app image
FROM mcr.microsoft.com/windows/servercore:ltsc2019

WORKDIR /save-prospect-handler
CMD .\SignUp.MessageHandlers.SaveProspect.exe

ENV Metrics:Enabled=true `
    Metrics:Port=50505    

COPY --from=builder C:\out\save-prospect\SaveProspectHandler .