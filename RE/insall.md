# Mô tả thử thách
<img width="724" height="414" alt="image" src="https://github.com/user-attachments/assets/45aeb18b-02c1-4f4c-8d63-123feecb2bb1" />

# Solution
Phân tích file bằng IDA, thấy có hàm main, ta phân tích hàm này trước
<pre lang="markdown">
    if ( (_BYTE)v4 )
    {
      v3 = 1;
    }
    else
    {
      std::string::substr(v25, v21, 3, -1);
      Seed = std::stoi(v25, 0, 10);
      std::string::~string(v25);
      generate_maze(Seed, (bool (*)[8][4])v19);
      std::operator<<<std::char_traits<char>>(&std::cout, "Enter moves: ");
      std::string::basic_string(v18);
      std::getline<char,std::char_traits<char>,std::allocator<char>>(&std::cin, v18);
      v34 = 0;
      v33 = 0;
      v32 = 20;
      v28 = v18;
      v16 = std::string::begin(v18);
      v15 = std::string::end(v28);
      while ( (unsigned __int8)__gnu_cxx::operator!=<char *,std::string>(&v16, &v15) )
      {
        v17 = *(_BYTE *)__gnu_cxx::__normal_iterator<char *,std::string>::operator*(&v16);
        v14 = std::map<char,std::pair<int,int>>::find(&v17);
        v26 = std::map<char,std::pair<int,int>>::end(&DIR);
        if ( (unsigned __int8)std::_Rb_tree_const_iterator<std::pair<char const,std::pair<int,int>>>::operator==(&v26) )
        {
          --v32;
        }
        else
        {
          v31 = -1;
          switch ( v17 )
          {
            case 'C':
              v31 = 0;
              break;
            case 'R':
              v31 = 1;
              break;
            case 'S':
              v31 = 2;
              break;
            case 'Y':
              v31 = 3;
              break;
          }
          if ( v31 >= 0 && *((_BYTE *)&v38[8 * v34 - 181 + v33] + v31) != 1 )
            --v32;
          v6 = std::_Rb_tree_const_iterator<std::pair<char const,std::pair<int,int>>>::operator->(&v14);
          v34 += *(_DWORD *)(v6 + 4);
          v7 = std::_Rb_tree_const_iterator<std::pair<char const,std::pair<int,int>>>::operator->(&v14);
          v33 += *(_DWORD *)(v7 + 8);
          if ( v34 >= 8 || v33 >= 8 )
          {
            v3 = 1;
            goto LABEL_41;
          }
        }
        if ( v17 == 65 || v17 == 73 )
          ++v32;
        if ( v32 <= 0 )
        {
          v3 = 1;
          goto LABEL_41;
        }
        __gnu_cxx::__normal_iterator<char *,std::string>::operator++(&v16);
      }
      if ( v34 == 7 && v33 == 7 )
      {
        std::string::substr(v27, v21, 3, -1);
        v8 = std::operator<<<std::char_traits<char>>(&std::cout, "Success! flag is VSL{");
        v9 = std::operator<<<char>(v8, v27);
        v10 = std::operator<<<std::char_traits<char>>(v9, "_");
        v11 = std::operator<<<char>(v10, v18);
        std::operator<<<std::char_traits<char>>(v11, "}");
        std::string::~string(v27);
        v3 = 0;
      }
      else
      {
        std::operator<<<std::char_traits<char>>(&std::cerr, "Did not reach exit!");
        v3 = 1;
      }
LABEL_41:
      std::string::~string(v18);
    }
    std::string::~string(v21);
    std::ostringstream::~ostringstream(v22);
  }
  std::string::~string(v23);
  return v3;
</pre>
Chương trình sẽ tạo ra 1 ma trận và yêu cầu người dùng nhập vào 1 chuỗi với các ký tự C, R, S, Y. Nếu đi đến ô có tọa độ 7,7 với điểu kiện là không được để stamina xuống 0 thì sẽ in ra ```bash "Success! flag is VSL{" ``` còn nếu sai thì sẽ in ra ```bash "Did not reach exit!"```
Vậy đầu tiên ta sẽ phân tích cách chương trình tạo ra ma trận qua hàm ```bash generate_maze```



