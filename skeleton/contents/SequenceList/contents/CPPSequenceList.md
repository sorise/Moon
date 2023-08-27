### [C++ 动态顺序表实现](#)

具体实现：
```cpp
template<typename ValueType>
class SequenceList final{
private:
    ValueType * _head_ptr, *_top_ptr ;
    //_top_ptr 指向最后构造元素之后的位置
    size_t size;
    size_t capacity;
    std::allocator<ValueType> alloc = std::allocator<ValueType>();
public:
    explicit SequenceList(size_t _capacity = 16):capacity(_capacity > 0?_capacity:16 ), size(0)
    {
        _head_ptr = alloc.allocate(_capacity);
        _top_ptr = _head_ptr;
    };
    SequenceList(const SequenceList& other) = delete;
    SequenceList(SequenceList&& other) noexcept {
        if (this != &other){
            _head_ptr = other._head_ptr;
            _top_ptr = other._top_ptr;
            size = other.size;
            capacity = other.capacity;

            //完成资源接管
            other._head_ptr = nullptr;
            other._top_ptr = nullptr;
            other.capacity = 0;
            other.size = 0;
        }
    };

    const ValueType* const_begin(){
        return _head_ptr;
    }

    const ValueType* const_end(){
        return _top_ptr;
    }

    template<typename ...Args>
    bool emplace_pushback(Args&&... args ) noexcept{
        try {
            if (size >= capacity){
                //扩大一倍
                auto _new_head = alloc.allocate( capacity * 2);
                auto _new_top = std::uninitialized_copy_n(_head_ptr, capacity, _new_head);
                while (_top_ptr != _head_ptr){
                    alloc.destroy(--_top_ptr); //call 析构函数
                }
                alloc.deallocate(_head_ptr, capacity); //释放
                //重新设置参数
                capacity *= 2;
                _head_ptr = _new_head;
                _top_ptr = _new_top;
            }
            alloc.construct(_top_ptr++, args...);
            size++;
        } catch (const std::bad_alloc & error) {
            return false;
        } catch (...) {
            return false;
        }
        return true;
    }

    [[nodiscard]] inline size_t get_size() const noexcept  { return size; }
    [[nodiscard]] inline size_t get_capacity() const noexcept  { return capacity; }

    virtual ~SequenceList(){
        while (_top_ptr != _head_ptr){
            alloc.destroy(--_top_ptr); //call 析构函数
        }
        //释放
        alloc.deallocate(_head_ptr, capacity);
    }

    ValueType& operator[](const size_t& index){
        if (index < size){
            return *(_head_ptr+index);
        }else{
            throw std::logic_error("Index of array out of bounds");
        }
    }

    bool removeByIndex(size_t index){
        if (index < size){
            auto _start_ptr = _head_ptr + index;
            alloc.destroy(_start_ptr); //call 析构函数
            //然后向前拷贝,ValueType需要支持拷贝
            while (_start_ptr!= _top_ptr){
                *_start_ptr = *(_start_ptr + 1); //赋值拷贝向前
                _start_ptr++;
            }
            size--;
            _top_ptr--;//减少一位
            return true;
        }else{
            throw std::logic_error("Index of array out of bounds");
        }
    }

};
```

测试代码:
```cpp
SequenceList<int> sl(5);

sl.emplace_pushback(20);
sl.emplace_pushback(21);
sl.emplace_pushback(23);
sl.emplace_pushback(24);
sl.emplace_pushback(25);
sl.emplace_pushback(26);
sl.emplace_pushback(27);
sl.emplace_pushback(30);
sl.emplace_pushback(31);
sl.emplace_pushback(32);
sl.emplace_pushback(33);
sl.emplace_pushback(34);

std::cout << "size: " << sl.get_size() << std::endl;
std::cout << "capacity:" <<sl.get_capacity() << std::endl;


sl[8] = 100;
sl.removeByIndex(2);
SequenceList<int> s2 = std::move(sl);
auto begin = s2.const_begin();

std::cout << "now size: " << s2.get_size() << std::endl;
std::cout << "now capacity:" <<s2.get_capacity() << std::endl;

while (begin != s2.const_end()){
    std::cout << *begin << " ";
    begin++;
}
```