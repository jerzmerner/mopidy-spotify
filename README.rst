**************
Mopidy-Spotify
**************

.. image:: https://img.shields.io/pypi/v/Mopidy-Spotify.svg?style=flat
    :target: https://pypi.python.org/pypi/Mopidy-Spotify/
    :alt: Latest PyPI version

.. image:: https://img.shields.io/pypi/dm/Mopidy-Spotify.svg?style=flat
    :target: https://pypi.python.org/pypi/Mopidy-Spotify/
    :alt: Number of PyPI downloads

.. image:: https://img.shields.io/travis/mopidy/mopidy-spotify/develop.png?style=flat
    :target: https://travis-ci.org/mopidy/mopidy-spotify
    :alt: Travis CI build status

.. image:: https://img.shields.io/coveralls/mopidy/mopidy-spotify/develop.svg?style=flat
   :target: https://coveralls.io/r/mopidy/mopidy-spotify?branch=develop
   :alt: Test coverage

`Mopidy <http://www.mopidy.com/>`_ extension for playing music from
`Spotify <http://www.spotify.com/>`_.


Dependencies
============

- A Spotify Premium subscription. Mopidy-Spotify **will not** work with Spotify
  Free, just Spotify Premium.

- A non-Facebook Spotify username and password. If you created your account
  through Facebook you'll need to create a "device password" to be able to use
  Mopidy-Spotify. Go to http://www.spotify.com/account/set-device-password/,
  login with your Facebook account, and follow the instructions.

- ``libspotify`` >= 12, < 13. The official C library from the `Spotify
  developer site <https://developer.spotify.com/technologies/libspotify/>`_.
  The package is available as ``libspotify12`` from
  `apt.mopidy.com <http://apt.mopidy.com/>`__.

- ``pyspotify`` >= 2. The ``libspotify`` python wrapper. The package is
  available as ``python-spotify`` from apt.mopidy.com or ``pyspotify`` on PyPI.

- ``Mopidy`` >= 0.18. The music server that Mopidy-Spotify extends.

If you install Mopidy-Spotify from apt.mopidy.com, AUR, or Homebrew, these
dependencies are installed automatically.


Installation
============

Debian/Ubuntu/Raspbian: Install the ``mopidy-spotify`` package from
`apt.mopidy.com <http://apt.mopidy.com/>`_::

    sudo apt-get install mopidy-spotify

Arch Linux: Install the ``mopidy-spotify`` package from
`AUR <https://aur.archlinux.org/packages/mopidy-spotify/>`_::

    yaourt -S mopidy-spotify

OS X: Install the ``mopidy-spotify`` package from the
`mopidy/mopidy <https://github.com/mopidy/homebrew-mopidy>`_ Homebrew tap::

    brew install mopidy-spotify

Else: Install the dependencies listed above yourself, and then install the
package from PyPI::

    pip install Mopidy-Spotify


Configuration
=============

Before starting Mopidy, you must add your Spotify Premium username and password
to your Mopidy configuration file::

    [spotify]
    username = alice
    password = secret

The following configuration values are available:

- ``spotify/enabled``: If the Spotify extension should be enabled or not.
  Defaults to ``true``.

- ``spotify/username``: Your Spotify Premium username. You *must* provide this.

- ``spotify/password``: Your Spotify Premium password. You *must* provide this.

- ``spotify/bitrate``: Audio bitrate in kbps. ``96``, ``160``, or ``320``.
  Defaults to ``160``.

- ``spotify/volume_normalization``: Whether volume normalization is active or
  not. Defaults to ``true``.

- ``spotify/timeout``: Seconds before giving up waiting for search results,
  etc. Defaults to ``10``.

- ``spotify/cache_dir``: The dir where the Spotify extension caches data.
  Defaults to ``$XDG_CACHE_DIR/mopidy/spotify``, which usually means
  ``~/.cache/mopidy/spotify``. If set to an empty string, caching is disabled.

- ``spotify/settings_dir``: The dir where the Spotify extension stores
  libspotify settings. Defaults to ``$XDG_CONFIG_DIR/mopidy/spotify``, which
  usually means ``~/.config/mopidy/spotify``.

- ``spotify/allow_network``: Whether to allow network access or not. Defaults
  to ``true``.

- ``spotify/allow_playlists``: Whether or not playlists should be exposed.
  Defaults to ``true``.

- ``spotify/search_album_count``: Maximum number of albums returned in search
  results. Number between 0 and 200. Defaults to 20.

- ``spotify/search_artist_count``: Maximum number of artists returned in search
  results. Number between 0 and 200. Defaults to 10.

- ``spotify/search_track_count``: Maximum number of tracks returned in search
  results. Number between 0 and 200. Defaults to 50.

- ``spotify/toplist_countries``: Comma separated list of two letter country
  domains to get toplists for. Defaults to a long list of countries which
  Spotify is available in.


Project resources
=================

- `Source code <https://github.com/mopidy/mopidy-spotify>`_
- `Issue tracker <https://github.com/mopidy/mopidy-spotify/issues>`_
- `Download development snapshot <https://github.com/mopidy/mopidy-spotify/tarball/develop#egg=Mopidy-Spotify-dev>`_


Changelog
=========

v2.0.0 (UNRELEASED)
-------------------

- Rewrite using pyspotify 2. Still left to reimplement for feature parity with
  Mopidy-Spotify 1.2.0:

  - Browsing of personal, country, and global top tracks.
  - Update playlists when they are changed by other clients.

**Config**

- Add ``spotify/volume_normalization`` config. (Fixes: #13)

- Add ``spotify/allow_network`` config which can be used to force
  Mopidy-Spotify to stay offline. This is mostly useful for testing during
  development.

- Add ``spotify/allow_playlists`` config which can be used to disable all
  access to playlists on the Spotify account. Useful where Mopidy is shared by
  multiple users. (Fixes: #25)

- Make maximum number of returned results configurable through
  ``spotify/search_album_count``, ``spotify/search_artist_count``, and
  ``spotify/search_track_count``.

**Lookup**

- Adding an artist by URI will now first find all albums by the artist and
  then all tracks in the albums. This way, the returned tracks are grouped by
  album and they are sorted by track number. (Fixes: #7)

- When adding an artist by URI, all albums that are marked as "compilations"
  or where the album artist is "Various Artists" are now ignored. (Fixes: #5)

**Playback**

- If another Spotify client starts playback with the same account, we get a
  "play token lost" event. Previously, Mopidy-Spotify would unconditionally
  pause Mopidy playback if this happened. Now, we only pause playback if we're
  currently playing music from Spotify. (Fixes: #1)


v1.2.0 (2014-07-21)
-------------------

- Add support for browsing playlists and albums. Needed to allow music
  discovery extensions expose these in a clean way.

- Fix loss of audio when resuming from paused, when caused by another Spotify
  client starting playback. (Fixes: #2, PR: #19)

v1.1.3 (2014-02-18)
-------------------

- Switch to new backend API locations, required by the upcoming Mopidy 0.19
  release.

v1.1.2 (2014-02-18)
-------------------

- Wait for track to be loaded before playing it. This fixes playback of tracks
  looked up directly by URI, and not through a playlist or search. (Fixes:
  mopidy/mopidy#675)

v1.1.1 (2014-02-16)
-------------------

- Change requirement on pyspotify from ``>= 1.9, < 2`` to ``>= 1.9, < 1.999``,
  so that it is parsed correctly and pyspotify 1.x is installed instead of 2.x.

v1.1.0 (2014-01-20)
-------------------

- Require Mopidy >= 0.18.

- Change ``library.lookup()`` to return tracks even if they are unplayable.
  There's no harm in letting them be added to the tracklist, as Mopidy will
  simply skip to the next track when failing to play the track. (Fixes:
  mopidy/mopidy#606)

- Added basic library browsing support that exposes user, global and country
  toplists.

v1.0.3 (2013-12-15)
-------------------

- Change search field ``track`` to ``track_name`` for compatibility with
  Mopidy 0.17. (Fixes: mopidy/mopidy#610)

v1.0.2 (2013-11-19)
-------------------

- Add ``spotify/settings_dir`` config value so that libspotify settings can be
  stored to another location than the libspotify cache. This also allows
  ``spotify/cache_dir`` to be unset, since settings are now using it's own
  config value.

- Make the ``spotify/cache_dir`` config value optional, so that it can be set
  to an empty string to disable caching.

v1.0.1 (2013-10-28)
-------------------

- Support searches from Mopidy that are using the ``albumartist`` field type,
  added in Mopidy 0.16.

- Ignore the ``track_no`` field in search queries, added in Mopidy 0.16.

- Abort Spotify searches immediately if the search query is empty instead of
  waiting for the 10s timeout before returning an empty search result.

v1.0.0 (2013-10-08)
-------------------

- Moved extension out of the main Mopidy project.
