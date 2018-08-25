### Tow Sum 的实现与优化

#### 问题描述（无序数组）

    给出一个数组，例如：nums = [2，7，11，15]，给出一个目标，target = 9。
    在数组中找出两个元素，是他们相加等于给的目标，返回两个元素的下标。
    例如：因为nums[0] + nums[1] = 2 + 7 = 9,return [0,1]。
    
#### 问题思考
    
    1. 最初考虑，因为要实现两个数相加等于给定的 target ，所以这两个数必定都小于 target。但是后来因为考虑到数组中可能出现负数，所以此条作废。
    2. 如果想让时间复杂度降低，就得尽量避免使用过多的 for 循环。
    
#### 代码实现

    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        for i in 0 ..< nums.count {
            for j in i+1 ..< nums.count {
                if nums[i]+nums[j] == target {
                    return [i,j]
                }
            }
        }
        return [Int]()
    }
    
#### 上述代码分析

    上述代码算是最简单的实现方式，两层 for 循环，一个一个的去试。但是这显然不是最好的解决方法，两层 for 循环占用了太多的时间。
    
#### 代码优化

    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var hashMap = [Int: Int]()
        var result = [Int]()
        
        for i in 0..<nums.count {
            let complement = target - nums[i] 
            
            if let index = hashMap[complement]{
                result.append(index)
                result.append(i)
                return result
            } else {     
                hashMap[nums[i]] = i
            } 
        }
        
        return result
    }
    
#### 代码分析

    1. 首先上述代码中只用了一个 for 循环，可以节约很多时间。
    2. 其次，用了一个暂存的 hsahMap 字典，用来存储结果和下标，如果 target - nums[i] 在这个 hashMap 里，则说明找到了这么一组数，如果没找到，则将其加入其中。
    
    
#### 附加问题（有序数组）

    上述问题给出的是一个无序的数组，如果这个数组有序呢？是不是可以继续优化它的空间复杂度呢？
    
#### 代码实现

    func twoSum(_ numbers: [Int], _ target: Int) -> [Int] {
            var begin = 0
            var end = numbers.count - 1
            while (begin < end) {
                let sum = numbers[begin] + numbers[end]
                if (sum == target) {
                    break
                } else if (sum < target) {
                    begin += 1
                } else {
                    end -= 1
                }
            }
            return [begin + 1, end + 1]
        }
    }
    
#### 代码分析

    在上述代码中，采用了begin 和 end 两个坐标，因为是有序的数组，所以将数组的begin 和 end 下标对应的元素相加，
    如果符合结果，直接 break，否则继续判断，如果 sum > target 说明 end 下标对应的元素数值过大，则 end-- 。
    否则 begin++。
    
#### 总结
    
    在实现一个算法问题的时候，首先考虑问题本身是否可以可以优化，进而考虑时间和空间复杂度，时间复杂度优先考虑，在时间复杂度很好的情况下，去优化空间复杂度。