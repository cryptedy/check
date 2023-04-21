文字配列の中の文字同士を連結するときをサンプルとしました。
sloadを検討に入れてるため各文字は31バイト未満を対象としています。

①通常だとuncheck + loop + concatが最適と考えます。
ただしガス代が高価です。（concatLoop）

②inline assemblyのmloadでのアプローチであれば、配列及び配列の値のlengthの取得は非常に楽ちんです。
一方でmemoryを広げてしまうのでガス代が高価になります。
（concatLoopMload、concatLoopMloadArgMemory）

③inline assemblyのsloadでのアプローチであれば、非常にガス代が安価になります。
とくに文字が固定長であればかなり安価です。
安価なまま可変長への対応ができないか悩んでいました。
（concatLoopSload、concatLoopSloadLength）

私が思いついたlengthの取得方法ですと
文字の長さが長くなるにつれて高価になり、
長さ10を超えるとmloadアプローチの方が安価になる傾向です。
