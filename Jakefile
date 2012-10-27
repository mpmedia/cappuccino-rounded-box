/*
 * Jakefile
 * RoundedBox Cappuccino
 *
 * Created by Alexander Ljungberg on October 12, 2012.
 * Copyright 2012, SlevenBits Ltd. All rights reserved.
 */

var ENV = require("system").env,
    FILE = require("file"),
    JAKE = require("jake"),
    task = JAKE.task,
    FileList = JAKE.FileList,
    app = require("cappuccino/jake").app,
    configuration = ENV["CONFIG"] || ENV["CONFIGURATION"] || ENV["c"] || "Debug",
    OS = require("os");

app ("RoundedBox Cappuccino", function(task)
{
    task.setBuildIntermediatesPath(FILE.join("Build", "RoundedBox Cappuccino.build", configuration));
    task.setBuildPath(FILE.join("Build", configuration));

    task.setProductName("RoundedBox Cappuccino");
    task.setIdentifier("com.slevenbits.cappuccino-rounded-box");
    task.setVersion("1.0");
    task.setAuthor("SlevenBits Ltd.");
    task.setEmail("feedback @nospam@ yourcompany.com");
    task.setSummary("RoundedBox Cappuccino");
    task.setSources((new FileList("**/*.j")).exclude(FILE.join("Build", "**")));
    task.setResources(new FileList("Resources/**"));
    task.setIndexFilePath("index.html");
    task.setInfoPlistPath("Info.plist");
    task.setNib2CibFlags("-R Resources/");

    if (configuration === "Debug")
        task.setCompilerFlags("-DDEBUG -g");
    else
        task.setCompilerFlags("-O");
});

task ("default", ["RoundedBox Cappuccino"], function()
{
    printResults(configuration);
});

task ("build", ["default"]);

task ("debug", function()
{
    ENV["CONFIGURATION"] = "Debug";
    JAKE.subjake(["."], "build", ENV);
});

task ("release", function()
{
    ENV["CONFIGURATION"] = "Release";
    JAKE.subjake(["."], "build", ENV);
});

task ("run", ["debug"], function()
{
    OS.system(["open", FILE.join("Build", "Debug", "RoundedBox Cappuccino", "index.html")]);
});

task ("run-release", ["release"], function()
{
    OS.system(["open", FILE.join("Build", "Release", "RoundedBox Cappuccino", "index.html")]);
});

task ("deploy", ["release"], function()
{
    FILE.mkdirs(FILE.join("Build", "Deployment", "RoundedBox Cappuccino"));
    OS.system(["press", "-f", FILE.join("Build", "Release", "RoundedBox Cappuccino"), FILE.join("Build", "Deployment", "RoundedBox Cappuccino")]);
    printResults("Deployment")
});

task ("desktop", ["release"], function()
{
    FILE.mkdirs(FILE.join("Build", "Desktop", "RoundedBox Cappuccino"));
    require("cappuccino/nativehost").buildNativeHost(FILE.join("Build", "Release", "RoundedBox Cappuccino"), FILE.join("Build", "Desktop", "RoundedBox Cappuccino", "RoundedBox Cappuccino.app"));
    printResults("Desktop")
});

task ("run-desktop", ["desktop"], function()
{
    OS.system([FILE.join("Build", "Desktop", "RoundedBox Cappuccino", "RoundedBox Cappuccino.app", "Contents", "MacOS", "NativeHost"), "-i"]);
});

function printResults(configuration)
{
    print("----------------------------");
    print(configuration+" app built at path: "+FILE.join("Build", configuration, "Capp"));
    print("----------------------------");
}
