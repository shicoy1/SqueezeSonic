# SqueezeSonic
===============

A Squeezeboxserver (Logitech Media Servver) plugin to connect to Subsonic API (>1.11.0) capable backends.

Do not forget to setup your credentials in the preferences pan. 

Have fun
----
Файл: SqueezeSonic/ProtocolHandler.pm

Место: функция getNextTrack, строки 117–120. Заменить блок

perl


		if ($format eq $track->{suffix}) {
			$format = "raw";
		}
		my $transcode = "format=" . $format;
на

perl


		my $trackFormat = $track->{suffix};
		$trackFormat = "flc" if $trackFormat eq "flac";
		if ($format eq $trackFormat) {
			$format = "raw";
		}
		my $apiFormat = $format;
		$apiFormat = "flac" if $apiFormat eq "flc";
		my $transcode = "format=" . $apiFormat;
Строка 129 $song->pluginData($format); остаётся без изменений — там нужен flc.


----



Файл: SqueezeSonic/ProtocolHandler.pm, функция getNextTrack, между строками 119 и 120.

Вставь две строки, третью замени:

perl


		if ($format eq $track->{suffix}) {
			$format = "raw";
		}
		my $apiFormat = $format;
		$apiFormat = "flac" if $apiFormat eq "flc";
		my $transcode = "format=" . $apiFormat;
Строку 129 $song->pluginData($format); не трогай — там должен остаться flc.

Про raw-ветку. Само по себе это её не оживит: строка 117 сравнивает flc с flac из $track->{suffix}. Чтобы заработало, замени условие на нормализованное сравнение:

perl


		my $trackFormat = $track->{suffix};
		$trackFormat = "flc" if $trackFormat eq "flac";
		if ($format eq $trackFormat) {
			$format = "raw";
		}
