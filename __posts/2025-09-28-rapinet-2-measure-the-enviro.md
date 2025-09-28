---
title: '.NET on Pi: Measure the environment with Enviro pHAT'
date: '2025-09-21'
---

Our journey with .NET on the Raspberry Pi continues. In the previous part of the series, we got an LED strip to blink; now we’ll read the current air temperature. But things turned out differently than expected.

Pimoroni Enviro pHAT
--------------------

The now unfortunately discontinued [Enviro pHAT from Pimoroni](https://learn.pimoroni.com/article/getting-started-with-enviro-phat) is a small add‑on board (a HAT) for the Raspberry Pi that combines several environmental sensors. With it you can measure temperature, air pressure, light, color, motion, and analog signals. In theory, that makes it ideal for small weather stations or room monitoring. An explanation—and a possible workaround—for a major drawback in real‑world use follows later in the article.

### BMP280

The BMP280 chip used on the HAT is a compact and versatile sensor from Bosch, widely used for environmental measurements. It precisely measures temperature and air pressure, making it ideal for weather stations, altitude measurements, or room monitoring. It works with a piezoresistive element whose electrical resistance changes with pressure. These subtle changes are processed digitally inside the sensor. With stored calibration data, the BMP280 delivers reliable and accurately reported values. Via interfaces like I²C or SPI, it connects easily to devices such as the Raspberry Pi or Arduino, providing a straightforward way to capture environmental data in your projects.

Entwicklung
-----------

### The plan

I’d love to say it was just as complex as our tinkering back in the Windows 10 IoT Core days on Dr. Windows—when we had to calculate and tweak calibration bit‑shuffles and compensation values ourselves, spending hours experimenting and always fearing we’d fry the chip with a wrong write command. And I actually felt like diving back in after all those years.

### Reality

I asked GitHub Copilot’s free tier for an implementation and provided the BMP280’s I²C address. After several steps and loops—where it recognized and fixed its own mistakes—I had, in about two minutes, a C# class that, using the official .NET NuGet packages `System.Device.Gpio` and `Iot.Device.Bindings`, read temperatures and output them more or less correctly formatted. Without me having to do anything else by hand. A bit scary.

So there isn’t much to explain here: thanks to the powerful .NET framework and GitHub Copilot, there was very little “original” work from me at this stage—and one shouldn’t adorn oneself with borrowed plumes.

### Finishing touches

Even the cleanup and documentation were largely possible with GitHub Copilot. Of course, my inner neat freak couldn’t resist hand‑tweaking a few things and renaming variables—but that’s likely on my boomer genes, not on Copilot’s quality.

Das Projekt
-----------

If you take a look at the [source code on GitHub](https://github.com/tscholze/dotnet-iot-raspberrypi-enviro), you’ll notice it does more than just read BMP280 sensor data. If you enjoy posts like this, I’m happy to continue with other sensors—or entirely different Raspberry Pi adventures. Let us know!