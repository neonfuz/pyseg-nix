{ lib
, stdenv

# development
, pkgs

, python311
, fetchFromGitHub
}:

let
  nuitka_ = python311.pkgs.callPackage ./nuitka.nix {};
in
python311.pkgs.buildPythonApplication rec {
  pname = "groundseg";
  version = "1.0.8";

  src = fetchFromGitHub {
    owner = "Native-Planet";
    repo = "GroundSeg";
    rev = "v${version}";
    hash = "sha256-OAc7r8hpH+m0HQIjahUGJNugI2SNgxZKtdso3OgRpAY=";
  };

  patchPhase = ''
    find api -type f -exec sed -i "s:/opt/nativeplanet:/var/lib:g" {} \;
  '';

  nativeBuildInputs = [
    nuitka_
  ];

  propagatedBuildInputs = with python311.pkgs; [
    chardet
    flask
    flask-cors
    requests
    docker
    #PyAccessPoint
    #asyncio
    websockets
    python-crontab
    psutil
    #pywgkey
    nuitka_
    zstandard
    ordered-set
    #nmcli
    cryptography
    schedule
  ];

  buildPhase = ''
    python3.11 -m nuitka --clang --onefile ./api/groundseg.py -o groundseg-bin --include-package=websockets --onefile-tempdir-spec="%TEMP%/groundseg/bin"
  '';

  preInstall = ''
    mkdir dist
    mv groundseg-bin dist/groundseg
  '';

  meta = with lib; {
    description = "The best way to run an Urbit ship";
    homepage = "https://github.com/Native-Planet/GroundSeg";
    license = licenses.mit;
    maintainers = with maintainers; [ neonfuz ];
  };
}
