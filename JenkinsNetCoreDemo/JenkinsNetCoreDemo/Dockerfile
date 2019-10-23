#FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS base
#WORKDIR /app
#EXPOSE 80
#EXPOSE 7010
#
#FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
#WORKDIR /src
#COPY ["JenkinsNetCoreDemo/JenkinsNetCoreDemo.csproj", "JenkinsNetCoreDemo/"]
#RUN dotnet restore "JenkinsNetCoreDemo/JenkinsNetCoreDemo.csproj"
#COPY . .
#WORKDIR "/src/JenkinsNetCoreDemo"
#RUN dotnet build "JenkinsNetCoreDemo.csproj" -c Release -o /app/build
#
#FROM build AS publish
#RUN dotnet publish "JenkinsNetCoreDemo.csproj" -c Release -o /app/publish
#
#FROM base AS final
#WORKDIR /app
#COPY --from=publish /app/publish .
#ENTRYPOINT ["dotnet", "JenkinsNetCoreDemo.dll"]
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS base
#注意.和/之间有空格,表示把主机当前目录内容拷贝到镜像的/publish目录
RUN ln -s /lib/x86_64-linux-gnu/libdl-2.24.so /lib/x86_64-linux-gnu/libdl.so

#开始安装系统必备组件
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
    echo "deb http://mirrors.163.com/debian/ stretch main non-free contrib" >/etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian/ stretch-proposed-updates main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch main non-free contrib" >>/etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch-proposed-updates main non-free contrib" >>/etc/apt/sources.list

RUN apt-get update
RUN apt-get install libfontconfig1 -y
RUN apt-get install libgdiplus -y && ln -s libgdiplus.so gdiplus.dll

FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
#系统必备组件安装完成
#ARG env
#run echo $env
COPY . /app
WORKDIR /app
#ENV ASPNETCORE_ENVIRONMENT=$env
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone
 
VOLUME /app
EXPOSE 7010
EXPOSE 80
 
ENTRYPOINT ["dotnet", "JenkinsNetCoreDemo.dll"]